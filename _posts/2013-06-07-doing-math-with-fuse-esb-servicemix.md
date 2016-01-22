---
layout: post
title: JBoss FUSE ESB vs. Apache ServiceMix vs. Apache Karaf vs. Apache Felix
date: '2013-06-07T11:53:00.001+02:00'
author: Peter Keller
tags:
- esb
modified_time: '2014-06-13T15:56:06.840+02:00'
thumbnail: http://1.bp.blogspot.com/-Nvb88Jaqm1M/UbzYJckIHlI/AAAAAAAAAlU/1H_N4l6ex4w/s72-c/framework_overview.png
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-1758968652103095495
blogger_orig_url: http://peter-on-java.blogspot.com/2013/06/doing-math-with-fuse-esb-servicemix.html
---

What is JBoss FUSE ESB? And what is its relation to OSGI, Apache Felix, Eclipse Equinox, Apache Karaf and Apache ServiceMix?

- **Felix** and **Equinox** are both OSGI core runtimes
- **Karaf** is the ServiceMix Kernel and provides a \"distribution\" based on Felix or Equinox by adding features such as an admin console and blueprint configuration.
- **ServiceMix** is an integration container aka ESB powered by OSGI unifying the features of ActiveMQ, Camel, CXF, and Karaf (and other)
- **JBoss Fuse ESB** is an ESB based on ServiceMix adding bug fixes and extended documentation

<a href="http://1.bp.blogspot.com/-Nvb88Jaqm1M/UbzYJckIHlI/AAAAAAAAAlU/1H_N4l6ex4w/s1600/framework_overview.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://1.bp.blogspot.com/-Nvb88Jaqm1M/UbzYJckIHlI/AAAAAAAAAlU/1H_N4l6ex4w/s400/framework_overview.png" height="270" width="400" /></a>

Sources:

- [http://felix.apache.org/](http://felix.apache.org/)
- [http://www.eclipse.org/equinox/](http://www.eclipse.org/equinox/)
- [http://karaf.apache.org/](http://karaf.apache.org/)
- [http://servicemix.apache.org/](http://servicemix.apache.org/)
- [http://www.redhat.com/products/jbossenterprisemiddleware/fuse/](http://www.redhat.com/products/jbossenterprisemiddleware/fuse/)
- [http://gnodet.blogspot.ch/2009/04/apache-karaf.html](http://gnodet.blogspot.ch/2009/04/apache-karaf.html)



