---
layout: post
title: 'Running Hadoop in a Docker Container on MacOS'
date: '2016-02-27T12:02:20.068+01:00'
author: Peter Keller
tags:
- docker
- macos
- hadoop
modified_time: '2016-02-27T12:02:20.068+01:00'
---

Instead of installing Hadoop directly on my MacBook, I decided to use a Docker
container in order to not polute my development environment. 

Fortunatelly, there is already one Docker image ready to install that can be found
on [https://github.com/sequenceiq/hadoop-docker](https://github.com/sequenceiq/hadoop-docker).


## Step 1: Download and install Docker infrastucture

Download the infrastucure:

- VirtualBox [https://www.virtualbox.org/](https://www.virtualbox.org/)
- Docker installer [https://docker.com/toolbox](https://docker.com/toolbox)

Double-click the DMG files and follow the instructions.


## Step 2: Create Docker machine
 
Use [docker-machine](https://docs.docker.com/machine/) to create default Docker machine for VirtualBox driver:

{% highlight sh %}
> docker-machine create --driver virtualbox default
{% endhighlight %}

Check if default Docker machine is created:

{% highlight sh %}
> docker-machine ls 
NAME      ACTIVE   DRIVER       STATE     URL   SWARM   DOCKER    ERRORS
default   *        virtualbox   Stopped                 Unknown   
{% endhighlight %}


## Step 3: Start Docker daemon

{% highlight sh %}
> docker-machine start default 
{% endhighlight %}

This starts:

- VirtualBox
- Lightweight Linux VM made specifically to run the Docker daemon on Mac OS X
- Docker daemon

Check status:

{% highlight sh %}
> docker-machine ls
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default   -        virtualbox   Running   tcp://192.168.99.100:2376           v1.10.2   
{% endhighlight %}


## Step 4: Init enviromental parameters

Init the Docker specific enviromental parameters in the shell where do you will build the Docker image:

{% highlight sh %}
> eval "$(docker-machine env default)"
{% endhighlight %}

Check the enviromental parameters:

{% highlight sh %}
> env | grep DOCKER
DOCKER_HOST=tcp://192.168.99.100:2376
DOCKER_MACHINE_NAME=default
DOCKER_TLS_VERIFY=1
DOCKER_CERT_PATH=/Users/peter/.docker/machine/machines/default
{% endhighlight %}


## Step 5: Clone the GIT project

{% highlight sh %}
> git clone https://github.com/sequenceiq/hadoop-docker.git
{% endhighlight %}

The `hadoop-docker` has been created. Step into it, for the further processing:

{% highlight sh %}
> cd hadoop-docker
{% endhighlight %}


## Step 6: Build the Docker image

Build the the Hadoop Docker image:

{% highlight sh %}
> docker build  -t sequenceiq/hadoop-docker:2.7.1 .
Sending build context to Docker daemon 229.9 kB
Step 1 : FROM sequenceiq/pam:centos-6.5
centos-6.5: Pulling from sequenceiq/pam
b253335dcf03: Pull complete 
a3ed95caeb02: Pull complete 
69623ef05416: Pull complete 
8d2023764774: Downloading [========================================>          ] 71.89 MB/88.09 MB
...
Successfully built 9e7e80f84015
{% endhighlight %}

Check if the image has been built and is registrated:

{% highlight sh %}
> docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
sequenceiq/hadoop-docker   2.7.1               9e7e80f84015        9 minutes ago       1.769 GB
{% endhighlight %}

Alternatively, you may just pull the image instead of building it yourself:

{% highlight sh %}
> docker pull sequenceiq/hadoop-docker:2.7.1
{% endhighlight %}


## Step 7: Run the Docker container

Run the Docker container:

{% highlight sh %}
> docker run -it sequenceiq/hadoop-docker:2.7.1 /etc/bootstrap.sh -bash
/
Starting sshd:                                             [  OK  ]
16/02/28 05:13:31 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Starting namenodes on [4c2d09c596b0]
4c2d09c596b0: starting namenode, logging to /usr/local/hadoop/logs/hadoop-root-namenode-4c2d09c596b0.out
localhost: starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-4c2d09c596b0.out
Starting secondary namenodes [0.0.0.0]
0.0.0.0: starting secondarynamenode, logging to /usr/local/hadoop/logs/hadoop-root-secondarynamenode-4c2d09c596b0.out
16/02/28 05:13:47 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
starting yarn daemons
starting resourcemanager, logging to /usr/local/hadoop/logs/yarn--resourcemanager-4c2d09c596b0.out
localhost: starting nodemanager, logging to /usr/local/hadoop/logs/yarn-root-nodemanager-4c2d09c596b0.out
bash-4.1# 
{% endhighlight %}

The bash shell is started and is ready to receive your commands.

Check the Java version:

{% highlight sh %}
bash-4.1# java -version
java version "1.7.0_71"
{% endhighlight %}


## Step 8: Check status of container from MacOS

{% highlight sh %}
> docker ps
CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS              PORTS                                                                                                                                                    NAMES
4c2d09c596b0        sequenceiq/hadoop-docker:2.7.1   "/etc/bootstrap.sh -b"   4 hours ago         Up 4 hours          2122/tcp, 8020/tcp, 8030-8033/tcp, 8040/tcp, 8042/tcp, 8088/tcp, 9000/tcp, 19888/tcp, 49707/tcp, 50010/tcp, 50020/tcp, 50070/tcp, 50075/tcp, 50090/tcp   compassionate_khorana
{% endhighlight %}


## Step 9: Test Hadoop: Grep

Go into the Hadoop directory:

{% highlight sh %}
bash-4.1# cd $HADOOP_PREFIX
bash-4.1# pwd
/usr/local/hadoop
{% endhighlight %}

Check the Hadoop version:

{% highlight sh %}
bash-4.1# ./bin/hadoop version
Hadoop 2.7.1
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r 15ecc87ccf4a0228f35af08fc56de536e6ce657a
Compiled by jenkins on 2015-06-29T06:04Z
Compiled with protoc 2.5.0
From source with checksum fc0a1a23fc1868e4d5ee7fa2b28a58a
This command was run using /usr/local/hadoop-2.7.1/share/hadoop/common/hadoop-common-2.7.1.jar
{% endhighlight %}

Run the mapreduce test:

{% highlight sh %}
bash-4.1# bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar grep input output 'dfs[a-z.]+'
16/02/27 05:54:51 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
16/02/27 05:54:52 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
16/02/27 05:54:53 INFO input.FileInputFormat: Total input paths to process : 31
...
{% endhighlight %}

Check the test output:

{% highlight sh %}
bash-4.1# bin/hdfs dfs -cat output/*
16/02/27 05:57:28 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
6	dfs.audit.logger
4	dfs.class
3	dfs.server.namenode.
2	dfs.period
2	dfs.audit.log.maxfilesize
2	dfs.audit.log.maxbackupindex
1	dfsmetrics.log
1	dfsadmin
1	dfs.servers
1	dfs.replication
1	dfs.file
{% endhighlight %}

List output directory:

{% highlight sh %}
bash-4.1# bin/hdfs dfs -ls         
16/02/28 06:05:09 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Found 2 items
drwxr-xr-x   - root supergroup          0 2016-02-28 05:03 input
drwxr-xr-x   - root supergroup          0 2016-02-28 06:04 output
{% endhighlight %}


## Step 10: Find more info

- VirtualBox: [https://www.virtualbox.org/](https://www.virtualbox.org/)
- Docker: [https://docker.com/](https://docker.com/)
- Docker installer: [https://docker.com/toolbox](https://docker.com/toolbox)
- Docker machine: [https://docs.docker.com/machine/](https://docs.docker.com/machine/)
- Hadoop Docker Image: [https://github.com/sequenceiq/hadoop-docker.git](https://github.com/sequenceiq/hadoop-docker.git)
- Hadoop: [http://hadoop.apache.org/](http://hadoop.apache.org/) 
