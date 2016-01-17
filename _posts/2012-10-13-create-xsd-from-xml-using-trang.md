---
layout: post
title: Create an XSD from XML using Trang
date: '2012-10-13T17:32:00.001+02:00'
author: Peter Keller
tags:
- XML
modified_time: '2012-10-13T17:33:41.562+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-8680870586419861372
blogger_orig_url: http://peter-on-java.blogspot.com/2012/10/create-xsd-from-xml-using-trang.html
---

I recently needed to create an XML Schema XSD file from an existing XML file. A short research pointed me to <a href="http://www.thaiopensource.com/relaxng/trang.html">Trang</a>. Description from their website:

>Trang, a program for converting between different schema languages, focused on RELAX NG; in particular, it can convert between the compact and XML syntaxes, it can convert between RELAX NG and DTDs, and it can convert from RELAX NG to W3C XML Schema

But beside the convertion between different schema languages, Trang is also able to create schemas based on XML files.

The creation of an XSD is done as follows for UTF-8 encoding using a JRE 1.5 or above:  

{% highlight sh %} 
java -jar trang.jar -i encoding=UTF-8 message.xml message.xsd
{% endhighlight %}

Or, with explicit input and output format if the former command does not work as expected:  

{% highlight sh %}
java -jar trang.jar -I xml -O xsd -i encoding=UTF-8 message.xml message.xsd
{% endhighlight %}

Easy.