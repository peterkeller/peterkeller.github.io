---
layout: post
title: JAVA_HOME setting on MacOS
date: '2012-10-22T22:10:00.000+02:00'
author: Peter Keller
tags:
- macos
modified_time: '2013-06-18T21:51:14.219+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-4031045522660758019
blogger_orig_url: http://peter-on-java.blogspot.com/2012/10/javahome-setting-on-macos.html
---

I just <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html">downloaded</a> the latest Java Update 1.7.09 for MacOS and installed it. And as usual, I had to fix the `JAVA_HOME` environmental property, so that all my Maven etc. tools still worked properly. In `~/.bash_profile`, I added/updated following entries: 

{% highlight sh %} 
export JAVA_HOME6=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
export JAVA_HOME7=/Library/Java/JavaVirtualMachines/jdk1.7.0_09.jdk/Contents/Home
export JAVA_HOME=$JAVA_HOME7
export PATH=$JAVA_HOME/bin:$PATH
{% endhighlight %}

I use `JAVA_HOME6` and `JAVA_HOME7` so that I can easily switch the standard Java version. 

But, stop, stop, there must be a better way to do that, so that I don\'t have to update 
these lines after every Java download. And actually it is by using the 
not-so-well-known-but-very-useful command `java_home`: 

> The java_home command returns a path suitable for setting the JAVA_HOME 
environment variable. It determines this path from the user\'s enabled and 
preferred JVMs in the Java Preferences application. Additional constraints 
may be provided to filter the list of JVMs available. [\...] The path is 
printed to standard output.

On my machine, `java_home` prints following path to the `STDOUT`: 

{% highlight sh %} 
[561]% /usr/libexec/java_home --version 1.7
/Library/Java/JavaVirtualMachines/jdk1.7.0_09.jdk/Contents/Home
{% endhighlight %}

So, I can change my `.bash_profile` as follows: 

{% highlight sh %} 
export JAVA_HOME6=`/usr/libexec/java_home --version 1.6`
export JAVA_HOME7=`/usr/libexec/java_home --version 1.7`
export JAVA_HOME=$JAVA_HOME7
export PATH=$JAVA_HOME/bin:$PATH
{% endhighlight %}

On my machine, my Java environmental parameters becomes: 

{% highlight sh %} 
[555]% env | grep JAVA
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_09.jdk/Contents/Home
JAVA_HOME6=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
JAVA_HOME7=/Library/Java/JavaVirtualMachines/jdk1.7.0_09.jdk/Contents/Home
{% endhighlight %}

Finito. 
