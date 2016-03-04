---
layout: post
title: 'Running Oracle XE in a Docker Container on MacOS'
date: '2016-03-04T12:02:20.068+01:00'
author: Peter Keller
tags:
- docker
- macos
- database
modified_time: '2016-03-04T12:02:20.068+01:00'
---

This post shows how to build and start Oracle XE on MacOS
using the Docker image provided by [https://github.com/wnameless/docker-oracle-xe-11g](https://github.com/wnameless/docker-oracle-xe-11g).

## Step 1: Start Docker

Start formerly installed `default` docker instance using `docker-machine`:

{% highlight sh %}
> docker-machine start default
{% endhighlight %}

Check: 

{% highlight sh %}
> docker-machine ls    
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v1.10.2   
{% endhighlight %}


## Step 2: Pull or build

Build on your own:
{% highlight sh %}
> git clone https://github.com/wnameless/docker-oracle-xe-11g
> cd docker-oracle-xe-11g
> 
{% endhighlight %}

Pull (lazy way):

{% highlight sh %}
> docker pull wnameless/oracle-xe-11g
{% endhighlight %}

## Step 3: Run

Run with 22 and 1521 ports opened:

{% highlight sh %}
>  docker run -d -p 49160:22 -p 49161:1521 wnameless/oracle-xe-11g
{% endhighlight %}

## Step 4: Test

Connect with DB client, e.g. SQLDeveloper, using default settings:

{% highlight text %}
hostname: ~~localhost~~ <IP>
port: 49161
sid: xe
username: system
password: oracle
{% endhighlight %}

Using `docker-machine` on MacOS, `localhost` does not work. Use the &lt;IP&gt; adress given by
following command instead:

{% highlight sh %}
> docker-machine ip
{% endhighlight %}

Connect with SSH to the container (password: admin), using the same &lt;IP&gt; adress as above:

{% highlight sh %}
> ssh root@~~localhost~~<IP> -p 49160    
{% endhighlight %}    

