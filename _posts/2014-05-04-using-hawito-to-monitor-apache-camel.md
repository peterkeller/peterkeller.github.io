---
layout: post
title: Using Hawtio to monitor Apache Camel route started as Java standalone process
date: '2014-05-04T20:27:00.002+02:00'
author: Peter Keller
tags:
- Monitoring
- Camel
- ESB
modified_time: '2014-06-05T07:40:25.467+02:00'
thumbnail: http://1.bp.blogspot.com/-r7rTYPYrUbM/U2aGSdeOxSI/AAAAAAAAAyc/0SjNSOVBelA/s72-c/hawito.jpg
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-2936295923705811339
blogger_orig_url: http://peter-on-java.blogspot.com/2014/05/using-hawito-to-monitor-apache-camel.html
---

## Step 1: Gathering info

[http://hawt.io/getstarted/index.html](http://hawt.io/getstarted/index.html)

## Step 2: Download Jolokia Java Agent

Download Jolokia JARs from [http://jolokia.org/dist/1.2.1/jolokia-1.2.1-bin.zip](http://jolokia.org/dist/1.2.1/jolokia-1.2.1-bin.zip). 
Unzip the file.

## Step 3: Add VM arguments to for enabling Jolokia Java Agent

Add following VM arguments when starting your Java standalone client:

{% highlight sh %}
-javaagent:/path/to/-javaagent:/.../jolokia-1.2.1/agents/jolokia-jvm.jar
{% endhighlight %}    

## Step 4: Start the Camel route

You should see something similar in your console

{% highlight sh %}
No access restrictor found, access to all MBean is allowed
Jolokia: Agent started with URL http://127.0.0.1:8778/jolokia/
{% endhighlight %}    

## Step 5: Download and install Chrom Client

Steps:

 1. Download the chrom extension from [http://central.maven.org/maven2/io/hawt/hawtio-crx/1.3.1/hawtio-crx-1.3.1.crx](http://central.maven.org/maven2/io/hawt/hawtio-crx/1.3.1/hawtio-crx-1.3.1.crx)
 2. Start Chrom and open extension path at `chrome://extensions/`
 3. Drop the downloaded CRX file onto the page
 4. Open the app page, select Hawtio app and enter the URL that was shown on Step 4. In my case this was port 8778 and path jolokia.

You should see the Hawtio page with your Camel route:

<a href="http://1.bp.blogspot.com/-r7rTYPYrUbM/U2aGSdeOxSI/AAAAAAAAAyc/0SjNSOVBelA/s1600/hawito.jpg" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://1.bp.blogspot.com/-r7rTYPYrUbM/U2aGSdeOxSI/AAAAAAAAAyc/0SjNSOVBelA/s1600/hawito.jpg" height="544" width="640" /></a>

I especially like the Diagram tab showing the route layout:

<a href="http://4.bp.blogspot.com/-0uHVGlKR-z4/U2aISl3F2HI/AAAAAAAAAyo/NeIwL5Ayo_Q/s1600/hawito-route.jpg" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://4.bp.blogspot.com/-0uHVGlKR-z4/U2aISl3F2HI/AAAAAAAAAyo/NeIwL5Ayo_Q/s1600/hawito-route.jpg" height="200" width="113" /></a>

