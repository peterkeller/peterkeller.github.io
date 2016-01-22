---
layout: post
title: How to set up Eclipse template for inserting Log4j Logger
date: '2012-06-18T12:30:00.003+02:00'
author: Peter Keller
tags:
- ide
modified_time: '2015-06-19T08:35:27.648+02:00'
thumbnail: http://2.bp.blogspot.com/-hERzSHPO80M/T98AdAuwUFI/AAAAAAAAAGg/FHkhqjXRXNk/s72-c/eclipse_log4j_template.png
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-1303711398984363010
blogger_orig_url: http://peter-on-java.blogspot.com/2012/06/eclipse-template-for-inserting-log4j.html
---

Tired adding <code>private static final Logger logger = Logger.getLogger(XXX.class);</code> by hand to your Java classes?

Use Eclipse templates instead. Navigate to Windows -> Preferences -> Java -> Editor -> Templates. And add following new template to the Java context:  

{% highlight java %} 
private static final Logger logger = Logger.getLogger(${enclosing_type}.class);
${:import(org.apache.log4j.Logger)}
{% endhighlight %} 

Finally, this should look as follows: 

<a href="http://2.bp.blogspot.com/-hERzSHPO80M/T98AdAuwUFI/AAAAAAAAAGg/FHkhqjXRXNk/s1600/eclipse_log4j_template.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://2.bp.blogspot.com/-hERzSHPO80M/T98AdAuwUFI/AAAAAAAAAGg/FHkhqjXRXNk/s320/eclipse_log4j_template.png" width="520" /></a>

Now, if you are in the Java context, then type \"logger\" followed by \"Ctrl+space\". You are invited to choose from different templates including your template entered above: 

<a href="http://2.bp.blogspot.com/-cQrmL2QbJVQ/T97-zqE7u-I/AAAAAAAAAGI/wb68eMJ9ghM/s1600/eclipse_log4j_select.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="320" src="http://2.bp.blogspot.com/-cQrmL2QbJVQ/T97-zqE7u-I/AAAAAAAAAGI/wb68eMJ9ghM/s320/eclipse_log4j_select.png" width="288" /></a>

After choosing the template, you ending up with an added logger plus the corresponding Java import: 

<a href="http://3.bp.blogspot.com/-rrD6pVGtNdI/T97-6aGlrUI/AAAAAAAAAGU/1YVA5vdD2Qk/s1600/eclipse_log4j_result.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="154" src="http://3.bp.blogspot.com/-rrD6pVGtNdI/T97-6aGlrUI/AAAAAAAAAGU/1YVA5vdD2Qk/s320/eclipse_log4j_result.png" width="320" /></a>

