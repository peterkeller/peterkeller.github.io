---
layout: post
title: Running Node.js in Docker
date: '2014-06-06T17:27:00.001+02:00'
author: Peter Keller
tags:
- Docker
modified_time: '2014-06-06T17:32:35.023+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-5116107384947732474
blogger_orig_url: http://peter-on-java.blogspot.com/2014/06/running-nodejs-in-docker.html
---

## 0. Inform

This is basically a copy of the Hello World example found [here](http://docs.docker.io/examples/nodejs_web_app/#nodejs-web-app)  and [here](https://github.com/gasi/docker-node-hello) with some additional comments. 

## 1. Make the VM running

{% highlight sh %}
    boot2docker up
{% endhighlight %}    

## 2. Build the Docker image 

Create your Node.js application [index.js](https://github.com/gasi/docker-node-hello/blob/master/index.js) file: 

{% highlight javascript %}
var express = require('express');
var DEFAULT_PORT = 8080;
var PORT = process.env.PORT || DEFAULT_PORT;
var app = express();
app.get('/', function (req, res) {
  res.send('Hello World\n');
});
app.listen(PORT)
console.log('Running on http://localhost:' + PORT);
{% endhighlight %}    

Create the Docker [package.json](https://github.com/gasi/docker-node-hello/blob/master/package.json) file: 

{% highlight json %}
{
  "name": "docker-centos-hello-world",
  "private": true,
  "version": "0.0.1",
  "description": "Node.js Hello World app on CentOS using docker",
  "author": "Daniel Gasienica",
  "dependencies": {
    "express": "3.2.4"
  }
}
{% endhighlight %}    

Create the [Dockerfile](https://github.com/gasi/docker-node-hello/blob/master/Dockerfile): 

{% highlight sh %}
FROM centos:6.4
RUN rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
RUN yum install -y npm
ADD . /src
RUN cd /src; npm install
EXPOSE 8080
CMD ["node", "/src/index.js"]
{% endhighlight %}    

The layout of your directory should be as follows:  

{% highlight sh %}
> ls
Dockerfile   index.js   package.json
{% endhighlight %}    

Building the image:

{% highlight sh %}
docker build -t peter/centos-node-hello .
{% endhighlight %}    

Verify if the image exists: 

{% highlight sh %}
docker images
{% endhighlight %}    

This should print: 

{% highlight sh %}
REPOSITORY                TAG                   IMAGE ID            CREATED             VIRTUAL SIZE
peter/centos-node-hello   latest                6c55823e6868        4 hours ago         672.2 MB
{% endhighlight %}    

## 3. Running 

{% highlight sh %}
docker run -p 49160:8080 -d peter/centos-node-hello
{% endhighlight %}    

The option `-p` exposes the port `8080` to `49160`. This is essential. Without that, the server is not visible to a client.

Verify, if the process is running:  

{% highlight sh %}
docker ps
{% endhighlight %}    

You should see:

{% highlight sh %}
CONTAINER ID        IMAGE                            COMMAND              CREATED             STATUS              PORTS                     NAMES
578bd01b0407        peter/centos-node-hello:latest   node /src/index.js   4 hours ago         Up 2 hours          0.0.0.0:49160->8080/tcp   jovial_feynman  
{% endhighlight %}    

Inspect the image:

{% highlight sh %}
docker inspect 578bd01b0407
{% endhighlight %}    

You should see a JSON output showing all relevant information:

{% highlight json %}
{
    "PortBindings": {
        "8080/tcp": [
            {
                "HostIp": "0.0.0.0",
                "HostPort": "49160"
            }
        ]
    }
}
{% endhighlight %}    

If the `-p` was not given when starting the process, `HostPort` would have been `null` and the application would not have been visible to clients. Test it:

{% highlight sh %}
curl localhost:49160
{% endhighlight %}    

Now you should see:

{% highlight sh %}
Hello World.
{% endhighlight %}    

Stop and start the process again:

{% highlight sh %}
docker stop 578bd01b0407
{% endhighlight %}    

docker `ps` doesn\'t show the process any more. `docker ps -a` lists the process as `Exited`:

{% highlight sh %}
> docker ps -a
CONTAINER ID        IMAGE                            COMMAND              CREATED             STATUS                       PORTS               NAMES
578bd01b0407        peter/centos-node-hello:latest   node /src/index.js   12 minutes ago      Exited (143) 9 seconds ago                       jovial_feynman     
{% endhighlight %}    

So, start it again:

{% highlight sh %}
docker start 578bd01b0407
{% endhighlight %}    

Test, if the process is running:

{% highlight sh %}
> docker -ps
CONTAINER ID        IMAGE                            COMMAND              CREATED             STATUS              PORTS                     NAMES
578bd01b0407        peter/centos-node-hello:latest   node /src/index.js   14 minutes ago      Up 2 seconds        0.0.0.0:49160->8080/tcp   jovial_feynman 
{% endhighlight %}    

Check the logs:

{% highlight sh %}
> docker logs 578bd01b0407
Running on http://localhost:8080
Running on http://localhost:8080
{% endhighlight %}    
