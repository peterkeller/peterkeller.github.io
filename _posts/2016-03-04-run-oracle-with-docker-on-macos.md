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


## Step 1: Start Docker

Start formerly installed `default` docker instance using `docker-machine`:

{% highlight sh %}
> docker-machine ls
NAME      ACTIVE   DRIVER       STATE   URL   SWARM   DOCKER    ERRORS
default   -        virtualbox   Saved                 Unknown   
> docker-machine start default
{% endhighlight %}

Check: 

{% highlight sh %}
> docker-machine ls    
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v1.10.2   
{% endhighlight %}

Set enviromnental parameters:

{% highlight sh %}
eval $(docker-machine env)
{% endhighlight %}

## Step 2: Pull or build

Build on your own:
{% highlight sh %}
> git clone https://github.com/wnameless/docker-oracle-xe-11g
> cd docker-oracle-xe-11g
> docker build  -t wnameless/oracle-xe-11g .
{% endhighlight %}

Check:
{% highlight sh %}
docker images     
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
wnameless/oracle-xe-11g    latest              45bd9967896a        About an hour ago   2.389 GB
{% endhighlight %}

Or just pull it from the repository (lazy way):

{% highlight sh %}
> docker pull wnameless/oracle-xe-11g
{% endhighlight %}

## Step 3: Run

Run the container with 22 and 1521 ports opened:

{% highlight sh %}
>  docker run -d -p 49160:22 -p 49161:1521 wnameless/oracle-xe-11g
{% endhighlight %}

## Step 4: Test

Connect to database with DB client, e.g. SQLDeveloper, using default settings:

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

