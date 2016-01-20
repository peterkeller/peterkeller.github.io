---
layout: post
title: How to set up Eclipse template for inserting SLF4J Logger
date: '2015-06-19T08:42:00.000+02:00'
author: Peter Keller
tags:
- IDE
modified_time: '2015-06-19T08:42:29.325+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-3594788144583946749
blogger_orig_url: http://peter-on-java.blogspot.com/2015/06/how-to-set-up-eclipse-template-for.html
---

Adding logger definition by hand to your Java classes is tiring:

{% highlight java %}
import org.slf4j.Logger
import org.slf4j.LoggerFactory

private static final Logger logger = LoggerFactory.getLogger(XXX.class);
{% endhighlight %}
    
Use Eclipse template. For SLF4J this looks as follows:  

{% highlight text %}
private static final Logger LOG = LoggerFactory.getLogger(${enclosing_type}.class);
${:import(org.slf4j.Logger,org.slf4j.LoggerFactory)}
{% endhighlight %}

 Note, that there are two import statements generated, one for the `Logger` and another for the `LoggerFactory` class. 
 
 The single steps needed to configure this template in the IDE, is explained in an older post [](here)<a href="http://peter-on-java.blogspot.ch/2012/06/eclipse-template-for-inserting-log4j.html">here</a>. <br/><br/>
