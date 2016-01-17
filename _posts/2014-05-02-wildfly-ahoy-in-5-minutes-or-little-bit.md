---
layout: post
title: JBoss Wildfly in 5 minutes (or a little bit more, but not much)
date: '2014-05-02T12:07:00.001+02:00'
author: Peter Keller
tags:
- Java EE
modified_time: '2016-01-15T09:59:46.768+01:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-1340055066418424567
blogger_orig_url: http://peter-on-java.blogspot.com/2014/05/wildfly-ahoy-in-5-minutes-or-little-bit.html
---

## Step 1: Gathering information

Access [http://www.wildfly.org/](http://www.wildfly.org/)

## Step 2: Download the server</h2>Download Wildfly v8.0.0:

Download from [http://download.jboss.org/wildfly/8.0.0.Final/wildfly-8.0.0.Final.zip](http://download.jboss.org/wildfly/8.0.0.Final/wildfly-8.0.0.Final.zip)

## Step 3: Install the server 

Unzip the downloaded zip file:

{% highlight sh  %}
unzip wildfly-8.0.0.Final.zip
{% endhighlight %}    

## Step 4: Start the server  

Go to the `bin` directory and start the server:

{% highlight sh  %}
> cd wildfly-8.0.0.Final/bin
> ./standalone.sh
{% endhighlight %}    

On my not so fast iMac, the first time the server was started, it took about 20 seconds, 
the second time only about 4 seconds:

{% highlight sh  %}
11:36:10,083 INFO   [org.jboss.ws.common.management] (MSC service thread 1-4) JBWS022052: Starting JBoss Web Services - Stack CXF Server 4.2.3.Final
11:36:10,153 INFO   [org.jboss.as] (Controller Boot Thread) JBAS015961: Http management interface listening on http://127.0.0.1:9990/management
11:36:10,154 INFO   [org.jboss.as] (Controller Boot Thread) JBAS015951: Admin console listening on http://127.0.0.1:9990
11:36:10,154 INFO   [org.jboss.as] (Controller Boot Thread) JBAS015874: WildFly 8.0.0.Final â€œWildFlyâ€ started in 4221ms - Started 183 of 232 services (80 services are lazy, passive or on-demand)
{% endhighlight %}    

Check, if the server is started on [http://localhost:8080](http://localhost:8080). You should see 
the Wildfly welcome page.

## Step 5: Access Administration console

Open [http://localhost:9990](http://localhost:9990).

This will not work and it states:

> To add a new user execute the add-user.sh script within the bin folder of your 
WildFly installation and enter the requested information.

Try that and answer the questions:

{% highlight sh  %}
> ./add-user.sh 
{% endhighlight %}    

I added an admin user. Access [http://localhost:9990](http://localhost:9990) again, and 
the admin cosole is shown.

## Step 6: Deploy the first application

Clone from GitHub:

{% highlight sh  %}
> git clone https://github.com/wildfly/quickstart.git
{% endhighlight %}    

Build and deploy the project:

{% highlight sh  %}
> cd quickstart/ 
> mvn wildfly:deploy
{% endhighlight %}    

You should see the following log messages:

{% highlight sh  %}
11:46:40,620 INFO   [org.jboss.as.repository] (management-handler-thread - 1) JBAS014900: Content added at location /Developer/Workspace/Java/containers/wildfly-8.0.0.Final/standalone/data/content/0b/37af36408419a89a9c1f015ea3570c87ced389/content
11:46:40,707 INFO   [org.jboss.as.server.deployment] (MSC service thread 1-1) JBAS015876: Starting deployment of "wildfly-helloworld.war" (runtime-name: "œwildfly-helloworld.war")
11:46:42,141 INFO   [org.jboss.weld.deployer] (MSC service thread 1-7) JBAS016002: Processing weld deployment wildfly-helloworld.war
11:46:42,468 INFO   [org.hibernate.validator.internal.util.Version] (MSC service thread 1-7) HV000001: Hibernate Validator 5.0.3.Final
11:46:42,915 INFO   [org.jboss.weld.deployer] (MSC service thread 1-4) JBAS016005: Starting Services for CDI deployment: wildfly-helloworld.war
11:46:43,031 INFO   [org.jboss.weld.Version] (MSC service thread 1-4) WELD-000900: 2.1.2 (Final)
11:46:43,207 INFO   [org.jboss.weld.deployer] (MSC service thread 1-5) JBAS016008: Starting weld service for deployment wildfly-helloworld.war
11:46:45,711 INFO   [org.wildfly.extension.undertow] (MSC service thread 1-1) JBAS017534: Registered web context: /wildfly-helloworld
11:46:45,860 INFO   [org.jboss.as.server] (management-handler-thread - 1) JBAS018559: Deployed "wildfly-helloworld.war" (runtime-name : "wildfly-helloworld.war"
{% endhighlight %}    

Access the application:

[http://localhost:8080/wildfly-helloworld/HelloWorld](http://localhost:8080/wildfly-helloworld/HelloWorld)

You should see the \"Hello World\!\" message.
