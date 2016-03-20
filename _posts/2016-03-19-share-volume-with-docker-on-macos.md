---
layout: post
title: 'Share volume in a Docker Container on MacOS'
date: '2016-03-19T12:02:20.068+01:00'
author: Peter Keller
tags:
- docker
- macos
modified_time: '2016-03-19T12:02:20.068+01:00'
---

This post shows how to share any MacOS volume in a Docker container. We use
docker-machine together with VirtualBox on MacOS. See older posts of mine
how this can be done.

## Share user home directory

Let's map a directory in the user home `/Users/peter/Hadoop`  to `/usr/local/hadoop/test-home`
in the Docker container. This should be easy, as the user directory is mounted
by boot2docker.

### Step 0: Setup

First initialize the directory and create a test file `test.txt` in it that can be
used to test if the mapping was successful:

{% highlight sh %}
> cd /Users/peter
> mkdir Hadoop
> cd Hadoop
> touch test.txt
> ls
test.txt
{% endhighlight %}

### Step 1: Map

Run Docker and map this directory to `/usr/local/hadoop/test-home` in the 
Docker container. For this we use the `-v` option as follows:

{% highlight sh %}
> docker run \
  -v /Users/peter/Hadoop:/usr/local/hadoop/test-home \ 
  -it sequenceiq/hadoop-docker:2.7.1 /etc/bootstrap.sh -bash
{% endhighlight %}    

Check Docker mounts:

{% highlight sh %}
> docker inspect web
...
    "Mounts": [
        {
            "Source": "/Users/peter/Hadoop",
            "Destination": "/usr/local/hadoop/test-home",
            "Mode": "",
            "RW": true,
            "Propagation": "rprivate"
        }
        ],
...       
{% endhighlight %}   

Check the home directory in boot2docker:

{% highlight sh %}
> docker-machine ssh default
...
Boot2Docker version 1.10.2, build master : 611be10 - Mon Feb 22 22:47:06 UTC 2016
Docker version 1.10.2, build c3959b1
docker@default:~$ ls /Users/peter/Hadoop/
test.txt
{% endhighlight %}

Check the directory in the Docker container:

{% highlight sh %}
bash-4.1# cd /usr/local/hadoop
bash-4.1# ls
LICENSE.txt  NOTICE.txt  README.txt ... test-home
bash-4.1# ls test-home/
test.txt
{% endhighlight %}    

`/usr/local/hadoop/test-home` was generated in the Docker container and we 
find the `test.txt` file. 

This was easy, the `-v` did the job.


## Share any directory

Mapping a directory in the user home was easy, as this directory is mounted
automatically in boot2docker. However, this is not the case for directories
outside of the user home directory as we will see below.

### Step 0: Setup

First, let's see what's in the MacOS `/Projects/hadoop` directory that we want to map:

{% highlight sh %}
> cd /Projects/hadoop
> ls
build/  build.gradle  settings.gradle  src/
{% endhighlight %}  

In our case, we are especially interested in the `build` directory with the 
generated Hadoop Map/Reduce libraries we want to use in the Hadoop Docker 
container. 

### Step 1: Map

Start Docker and map the project directory `/Projects/hadoop` to 
`/usr/local/hadoop/test-project` as above:

{% highlight sh %}
docker run \
  -v /Users/peter/Hadoop:/usr/local/hadoop/test-home \ 
  -v /Projects/hadoop:/usr/local/hadoop/test-project  \
  -it sequenceiq/hadoop-docker:2.7.1 /etc/bootstrap.sh -bash
{% endhighlight %}    

Check Docker mounts:

{% highlight sh %}
> docker inspect web
...
    "Mounts": [
        {
            "Source": "/Users/peter/Hadoop",
            "Destination": "/usr/local/hadoop/test-home",
            "Mode": "",
            "RW": true,
            "Propagation": "rprivate"
        },
        {
            "Source": "/Projects/hadoop-test",
            "Destination": "/usr/local/hadoop/test-project",
            "Mode": "",
            "RW": true,
            "Propagation": "rprivate"
        }
        ],
...     
{% endhighlight %}    

This looks as expected. 

List the mapped volume in the Docker container:

{% highlight sh %}
bash-4.1# cd /usr/local/hadoop/test-home
bash-4.1# ls
LICENSE.txt  NOTICE.txt  README.txt ... 	test-home  test-project
bash-4.1# ls test-project
bash-4.1# 
{% endhighlight %}    

The directory was generated in the Docker container but it is empty! 

The reason for this is as mentioned above, that only the user home directory
is mounted in boot2docker per default and not any other. 

Two additional steps are needed:

- Map the volume in VirtualBox
- Mount the volume in boot2docker

### Step 2: Map the volume in VirtualBox

Stop VirtualBox with `docker-machine stop default` and share the 
`/Projects/hadoop` folder with:

{% highlight sh %}
VBoxManage sharedfolder add default --name "hadoop" --hostpath /Projects/hadoop 
{% endhighlight %}

Start the VirtualBox again with `docker-machine start default`.

Alternatively, you may use the VirtualBox UI (Settings -> Shared Folders).

### Step 3: Mount the volume in boot2docker:

Use `mount` to mount the MacOS directory in boot2docker. 

Usage:

{% highlight sh %}
docker@default:~$ mount --help
BusyBox v1.23.1 (2015-02-22 15:52:11 UTC) multi-call binary.
Usage: mount [OPTIONS] [-o OPTS] DEVICE NODE
...
{% endhighlight %}

where:

- `DEVICE` is the logical name of the shared folder defined in VirtualBox
- `NODE` is the hostpath of the directory to be shared in boot2docker (not MacOS!)

Use it:

{% highlight sh %}
> docker-machine ssh default
docker@default:~$ sudo mkdir -p /hadoop
docker@default:~$ sudo mount -t vboxsf -o defaults,uid=`id -u docker`,gid=`id -g docker` \
  hadoop /hadoop
docker@default:~$ ls /hadoop
build/  build.gradle  settings.gradle  src/  
{% endhighlight %}

Note, that `DEVICE` must correspond to the logical name of the shared folder defined
in VirtualBox above. If you missed the name, then following error message is returned:

{% highlight sh %}
docker@default:~$ sudo mount -t vboxsf -o defaults,uid=`id -u docker`,gid=`id -g docker` \
  wrong-logical-name /Projects/hadoop
mount.vboxsf: mounting failed with the error: Protocol error
{% endhighlight %}

`NODE` must correspond an existing file path in boot2docker (not MacOS!). If you missed this name, then
following error message is returned:

{% highlight sh %}
docker@default:~$ sudo mount -t vboxsf -o defaults,uid=`id -u docker`,gid=`id -g docker` \
  projects-git /wrong-name
mount.vboxsf: mounting failed with the error: No such file or directory
mount: mounting projects-git on /wrong-name failed: No such file or directory
{% endhighlight %}

Run the Docker container again as above:

{% highlight sh %}
> docker run \
  -v /hadoop:/usr/local/hadoop/test-project  \
  -it sequenceiq/hadoop-docker:2.1 /etc/bootstrap.sh -bash
{% endhighlight %}  

Note, that `/hadoop` corresponds to `NODE` set above (not the original path in MacOS!). 

List the project directory in the Docker container:

{% highlight sh %}
bash-4.1# cd /usr/local/hadoop
bash-4.1# ls
LICENSE.txt  NOTICE.txt  README.txt ...  test-project
bash-4.1# ls test-project
build  build.gradle  settings.gradle  src
{% endhighlight %}    

Yes, it works. However, if you restart boot2docker, the mounted volumes disappear.

### Step 4: Permanently mount MacOS volumes in boot2docker

In boot2docker, edit/create (as root) `/mnt/sda1/var/lib/boot2docker/bootlocal.sh` to
add an automount:  

{% highlight sh %}
mkdir -p /hadoop                         
mount -t vboxsf -o defaults,uid=`id -u docker`,gid=`id -g docker` hadoop /hadoop  
{% endhighlight %}    
    
## More information

See this [StackOverflow](http://stackoverflow.com/questions/30040708/how-to-mount-local-volumes-in-docker-machine)
post.
 