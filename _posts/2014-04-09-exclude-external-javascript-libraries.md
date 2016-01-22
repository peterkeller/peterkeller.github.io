---
layout: post
title: Exclude external JavaScript libraries from beeing validated in Eclipse 4
date: '2014-04-09T17:30:00.002+02:00'
author: Peter Keller
tags:
- ide
modified_time: '2014-04-09T17:30:54.999+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-8406368211509145182
blogger_orig_url: http://peter-on-java.blogspot.com/2014/04/exclude-external-javascript-libraries.html
---

Due to an Eclipse Bug, JavaScript errors of external JavaScript libraries are marked as erroronous showing the red failure marker even if JavaScript errors are excluded from the contents Markers view, see [https://bugs.eclipse.org/bugs/show_bug.cgi?id=349020](https://bugs.eclipse.org/bugs/show_bug.cgi?id=349020)

Workaround as spooted in the bug history:

> I have found that I can leave the JavaScript Validator enable and ignore 
    specific files by adding a suitable exclusion pattern e.g. **/jquery*.js to 
    the JavaScript/Include Path/Source/Excluded group (Project->Properties->JavaScript->Include Path->Source).

