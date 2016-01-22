---
layout: post
title: Running Docker on MacOS
date: '2014-06-06T10:10:00.000+02:00'
author: Peter Keller
tags:
- docker
- macos
modified_time: '2014-06-06T15:03:50.812+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-8808282751460316344
blogger_orig_url: http://peter-on-java.blogspot.com/2014/06/running-docker-on-macos.html
---


## Inform

Almost all information about installing docker on MacOS can be found 
[here](http://docs.docker.io/installation/mac/).

## Install VirtualBox

Download VirtualBox from [here](https://www.virtualbox.org/wiki/Downloads). 
Double-click the `*.dmg` file and install the application following the install dialog.

## Install boot2docker

boot2docker is used to manage the docker VMs. The installer for MacOS can be found 
[here](https://github.com/boot2docker/osx-installer/releases). Double-click the 
`Docker.dmg` file and install the application following the install dialog.

## Install Docker client

Installation routine:

{% highlight sh %}
> mkdir tmp
> cd tmp
> curl -f -o ./ld.tgz https://get.docker.io/builds/Darwin/x86_64/docker-latest.tgz
> gunzip ld.tgz 
> tar xvf ld.tar 
> sudo cp usr/local/bin/docker /usr/local/bin
{% endhighlight %}    

Specify the docker deamon Host for the client:

{% highlight sh %}
export DOCKER_HOST=tcp://127.0.0.1:4243
{% endhighlight %}    

## Initialize boot2docker VM and run deamon

Initialize the VM:

{% highlight sh %}
> boot2docker init
2014/06/06 09:51:37 Downloading boot2docker ISO image...
2014/06/06 09:51:38 Latest release is v0.9.1
2014/06/06 09:52:01 Success: downloaded https://github.com/boot2docker/boot2docker/releases/download/v0.9.1/boot2docker.iso
    to /Users/peterkeller/.boot2docker/boot2docker.iso
Generating public/private rsa key pair.
...
2014/06/06 09:53:03 Creating VM boot2docker-vm...
2014/06/06 09:53:04 Apply interim patch to VM boot2docker-vm (https://www.virtualbox.org/ticket/12748)
2014/06/06 09:53:04 Setting NIC #1 to use NAT network...
2014/06/06 09:53:04 Port forwarding [ssh] tcp://127.0.0.1:2022 --> :22
2014/06/06 09:53:04 Port forwarding [docker] tcp://127.0.0.1:4243 --> :4243
2014/06/06 09:53:04 Setting NIC #2 to use host-only network "vboxnet0"...
2014/06/06 09:53:04 Setting VM storage...
2014/06/06 09:53:10 Done. Type `boot2docker up` to start the VM.
{% endhighlight %}    

Running the deamon:

{% highlight sh %}
> boot2docker up
2014/06/06 09:55:23 Waiting for SSH server to start...
2014/06/06 09:55:47 Started.
2014/06/06 09:55:47 To connect the Docker client to the Docker daemon, please set:
2014/06/06 09:55:47     export DOCKER_HOST=tcp://localhost:4243
{% endhighlight %}    

Test:

{% highlight sh %}
> docker version
Client version: 0.11.1
Client API version: 1.11
Go version (client): go1.2.1
Git commit (client): fb99f99
Server version: 0.11.1
Server API version: 1.11
Git commit (server): fb99f99
Go version (server): go1.2.1
{% endhighlight %}    

Setup forward network ports. According to the documentation, the boot2docker VM must be powered off for this to work:

{% highlight sh %}
> boot2docker stop
{% endhighlight %}    

Run following script (this takes a while):

{% highlight sh %}
for i in {49000..49900}; do
 VBoxManage modifyvm "boot2docker-vm" --natpf1 "tcp-port$i,tcp,,$i,,$i";
 VBoxManage modifyvm "boot2docker-vm" --natpf1 "udp-port$i,udp,,$i,,$i";
done
{% endhighlight %}    

Starting docker VM again and login using ssh:

{% highlight sh %}
> boot2docker up
> boot2docker ssh
{% endhighlight %}    

Then you should see:

{% highlight sh %}
                        ##        .
                  ## ## ##       ==
               ## ## ## ##      ===
           /""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
           \______ o          __/
             \    \        __/
              \____\______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_| 
{% endhighlight %}    
