---
layout: post
title: Migrating Plug-in Configuration when updating Eclipse Release
date: '2013-04-21T20:08:00.001+02:00'
author: Peter Keller
tags:
- ide
modified_time: '2013-06-18T21:56:02.534+02:00'
thumbnail: http://2.bp.blogspot.com/-05dgt6G2DXo/UHFaZkoYXII/AAAAAAAAAHE/PB7JK-3VZ6U/s72-c/eclipse-export.png
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-3592347888813412781
blogger_orig_url: http://peter-on-java.blogspot.com/2013/04/migrating-plug-ins-when-updating.html
---

I just downloaded the latest Eclipse Juno Release with the latest fixes etc. But how to migrate my plugins from my \"old\" Eclipse to the \"new\" one?    

## 1. Export

First, you must export your plug-ins from your \"old\" Eclipse installation as follows: 

1.0 Start your \"old\" Eclipse instance 

1.1 Package explorer, right mouse click, choose Export  

<a href="http://2.bp.blogspot.com/-05dgt6G2DXo/UHFaZkoYXII/AAAAAAAAAHE/PB7JK-3VZ6U/s1600/eclipse-export.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="288" src="http://2.bp.blogspot.com/-05dgt6G2DXo/UHFaZkoYXII/AAAAAAAAAHE/PB7JK-3VZ6U/s320/eclipse-export.png" width="320" /></a>

1.2 Choose Install: Installed Software Items to File  

<a href="http://3.bp.blogspot.com/-iP6SrULcy0g/UHFajDWXTnI/AAAAAAAAAHQ/x9vWGRE6B20/s1600/eclipse-export-to-file.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="143" src="http://3.bp.blogspot.com/-iP6SrULcy0g/UHFajDWXTnI/AAAAAAAAAHQ/x9vWGRE6B20/s320/eclipse-export-to-file.png" width="320" /></a>

1.3 Select All and export to <i>p2f</i> file (remember the file path, as you will import this file into your \"new\" Eclipse instance)  

<a href="http://4.bp.blogspot.com/-zNNr61WVL5U/UHFlU4dNKJI/AAAAAAAAAHk/vVNyw0LRHhQ/s1600/eclipse-export-select-all.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="320" src="http://4.bp.blogspot.com/-zNNr61WVL5U/UHFlU4dNKJI/AAAAAAAAAHk/vVNyw0LRHhQ/s320/eclipse-export-select-all.png" width="253" /></a>

## 2. Import

After exporting, you must import the exported <i>p2f</i> file to your \"new\" Eclipse installation as follows: 

2.0 Shutdown the \"old\" Eclipse instance if you want to use the same workspace as used for the export and start your \"new\" Eclipse instance 

2.1 Package explorer, right mouse click, choose Import  

<a href="http://3.bp.blogspot.com/-N62Yg0YhjCI/UHFl89c4eVI/AAAAAAAAAHw/cHl0SMfJhbw/s1600/eclipse-import.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="291" src="http://3.bp.blogspot.com/-N62Yg0YhjCI/UHFl89c4eVI/AAAAAAAAAHw/cHl0SMfJhbw/s320/eclipse-import.png" width="320" /></a>

2.2 Choose Install: Install Software Items from File  

<a href="http://2.bp.blogspot.com/-wS4C9bbJXZ8/UHFmNc3d1PI/AAAAAAAAAH8/Xr4tGAGxW8w/s1600/eclipse-import-from-file.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="320" src="http://2.bp.blogspot.com/-wS4C9bbJXZ8/UHFmNc3d1PI/AAAAAAAAAH8/Xr4tGAGxW8w/s320/eclipse-import-from-file.png" width="305" /></a>

2.3 Select your exported <i>p2f</i> file (see step 1.3 above) 

2.4 Select All and Finish  

<a href="http://4.bp.blogspot.com/-rsexvSWGkuI/UHFmdZ_HnVI/AAAAAAAAAII/uDJY--H1ims/s1600/eclipse-import-select-all.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="257" src="http://4.bp.blogspot.com/-rsexvSWGkuI/UHFmdZ_HnVI/AAAAAAAAAII/uDJY--H1ims/s320/eclipse-import-select-all.png" width="320" /></a>

That\'s it.
