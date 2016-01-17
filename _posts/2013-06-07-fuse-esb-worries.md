---
layout: post
title: JBoss FUSE ESB / Apache ServiceMix Worries...
date: '2013-06-07T17:31:00.002+02:00'
author: Peter Keller
tags:
- ESB
modified_time: '2013-06-23T21:17:05.831+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-5862836593653017471
blogger_orig_url: http://peter-on-java.blogspot.com/2013/06/fuse-esb-worries.html
---

I started JBOSS FUSE ESB and received a million error messages in the log (short extract): 

{% highlight sh %} 
[510]% ./fuse
Please wait while JBoss Fuse is loading...
 29% [====================&gt;  ]ERROR: Bundle org.ops4j.pax.web.pax-web-spi [97] Error starting mvn:org.ops4j.pax.web/pax-web-spi/1.1.11 (org.osgi.framework.BundleException: Uses constraint violation. Unable to resolve bundle revision org.ops4j.pax.web.pax-web-spi [97.0] because it is exposed to package 'javax.servlet' from bundle revisions org.apache.geronimo.specs.geronimo-servlet_3.0_spec [269.0] and org.mortbay.jetty.servlet-api [263.0] via two dependency chains.
Chain 1:
  org.ops4j.pax.web.pax-web-spi [97.0]
  import: (&amp;(osgi.wiring.package=javax.servlet)(version&gt;=2.3.0)(!(version&gt;=3.0.0)))
  |
  export: osgi.wiring.package=javax.servlet
  org.apache.geronimo.specs.geronimo-servlet_3.0_spec [269.0]
Chain 2:
  org.ops4j.pax.web.pax-web-spi [97.0]
  import: (&amp;(osgi.wiring.package=org.ops4j.pax.web.service)(version&gt;=1.1.11))
  |
  export: osgi.wiring.package=org.ops4j.pax.web.service; uses:=org.osgi.service.http
  org.ops4j.pax.web.pax-web-api [99.0]
  import: (&amp;(osgi.wiring.package=org.osgi.service.http)(version&gt;=1.0.0)(!(version&gt;=2.0.0)))
  |
  export: osgi.wiring.package=org.osgi.service.http; uses:=javax.servlet.http
  osgi.cmpn [273.0]
  import: (osgi.wiring.package=javax.servlet.http)
  |
  export: osgi.wiring.package=javax.servlet.http; uses:=javax.servlet
  export: osgi.wiring.package=javax.servlet
  org.mortbay.jetty.servlet-api [263.0])
  ...
{% endhighlight %}   

WTF? I deleted `$FUSE_HOME/data` directory, started FUSE again, 
and the errors have been gone. 

It\'s a pain.

