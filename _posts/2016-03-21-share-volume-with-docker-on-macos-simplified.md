---
layout: post
title: 'Share volumes in a Docker Container on MacOS - simplified'
date: '2016-03-21T12:02:20.068+01:00'
author: Peter Keller
tags:
- docker
- macos
modified_time: '2016-03-21T12:02:20.068+01:00'
---

In the last [post](2016/03/19/share-volume-with-docker-on-macos/) I showed how to 
share a volume in a Docker container on MacOS. There was a way, but it was not 
really easy. 

Alternatively, there is another way that's much easier: use 
[docker-machine-fs](https://github.com/adlogix/docker-machine-nfs).


## Step 1: Install docker-machine-nfs

Standalone:

{% highlight sh %}
curl -s https://raw.githubusercontent.com/adlogix/docker-machine-nfs/master/docker-machine-nfs.sh |
  sudo tee /usr/local/bin/docker-machine-nfs > /dev/null && \
  sudo chmod +x /usr/local/bin/docker-machine-nfs
{% endhighlight %}  
  
Or using Homewbrew:
  
{% highlight sh %}
brew install docker-machine-nfs
{% endhighlight %}  


## Step 2: Map the volume in VirtualBox and permanently mount the volume in boot2docker

{% highlight sh %}
> docker-machine-nfs default --shared-folder=/Projects/hadoop
[INFO] Configuration:

	- Machine Name: default 
	- Shared Folder: /Projects/hadoop 
	- Mount Options: noacl,async 
	- Force: false 

[INFO] machine presence ...                  OK 
[INFO] machine running ...                   OK 
[INFO] Lookup mandatory properties ...       OK 

	- Machine IP: 192.168.99.100 
	- Network ID: vboxnet2 
	- NFSHost IP: 192.168.99.1 

[INFO] Configure NFS ... 

 !!! Sudo will be necessary for editing /etc/exports !!! 
The nfsd service does not appear to be running.
Starting the nfsd service
                                             OK 
[INFO] Configure Docker Machine ...          OK 
[INFO] Restart Docker Machine ...            OK 
[INFO] Verify NFS mount ... 			


OK 

--------------------------------------------

 The docker-machine 'default'
 is now mounted with NFS!

 ENJOY high speed mounts :D

--------------------------------------------
{% endhighlight %}  

As seen in the log, sudo is needed to create/update `/etc/exports`.
Content:

{% highlight sh %}
/Projects/hadoop 192.168.99.100 -alldirs -mapall=501:20
{% endhighlight %}

Note, that in boot2docker the file `/mnt/sda1/var/lib/boot2docker/bootlocal.sh` 
is overridden:

{% highlight sh %}
docker@default:~$ cat /mnt/sda1/var/lib/boot2docker/bootlocal.sh
#!/bin/sh
sudo umount /Users
sudo mkdir -p /Projects/hadoop
sudo /usr/local/etc/init.d/nfs-client start
sudo mount -t nfs -o noacl,async 192.168.99.1:/Projects/hadoop /Projects/hadoop
{% endhighlight %}

I.e., `/Projects/hadoop` in MacOS is mapped to `/Projects/hadoop` in boot2docker.

Test it:

{% highlight sh %}
> docker-machine ssh default
docker@default:~$  ls /Projects/hadoop
build/  build.gradle  settings.gradle  src/
{% endhighlight %} 

## Step 3: Map the volumes

Run the Docker container:

{% highlight sh %}
> docker run \
  -v /Projects/hadoop:/usr/local/hadoop/test-project  \
  -it sequenceiq/hadoop-docker:2.1 /etc/bootstrap.sh -bash
{% endhighlight %}  

List the project directory in the Docker container:

{% highlight sh %}
bash-4.1# cd /usr/local/hadoop
bash-4.1# ls
LICENSE.txt  NOTICE.txt  README.txt ...  test-project
bash-4.1# ls test-project
build  build.gradle  settings.gradle  src
{% endhighlight %}     

It works.
