---
layout: post
title: ' Setup AppDynamics for JBoss FUSE ESB / Apache ServiceMix in 5 Minutes'
date: '2013-06-15T22:16:00.000+02:00'
author: Peter Keller
tags:
- monitoring
- esb
modified_time: '2013-06-23T21:16:47.499+02:00'
thumbnail: http://1.bp.blogspot.com/-dwT6fA6c6Qc/UbzItTFSMXI/AAAAAAAAAlE/fZax_LgVWl4/s72-c/appdynamics_overview.png
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-7313073903244805374
blogger_orig_url: http://peter-on-java.blogspot.com/2013/06/setup-appdynamics-for-fuse-esb-in-5.html
---

<a href="http://www.appdynamics.com/" target="_blank">AppDynamics</a> is a powerful tool for 
the analysis of distributed Java and .NET applications. As it has only low overhead 
costs (according to the vendor < 2%), it can also be used in production environments.

I use JBoss Fuse 6.0.0 on MacOS. But the setup is similar on other operation systems. 

Here we go.

Download AppDynamics Lite Java version from <a href="http://www.appdynamics.com/">http://www.appdynamics.com/</a>. 
 Name and E-Mail address must be provided (and normally you will be contacted by the vendor\...).

Unzip the downloaded ZIP file `AppDynamicsLite.zip` to your desired installation 
 directory `<APP_DYNAMICS_HOME>`: 
 
{% highlight sh %} 
unzip AppDynamicsLite.zip -d <APP_DYNAMICS_HOME>;
{% endhighlight %} 

Go to the installation directory:
{% highlight sh %} 
cd <APP_DYNAMICS_HOME>;
{% endhighlight %} 

Unzip the viewer package:
{% highlight sh %} 
unzip LiteViewer.zip
{% endhighlight %} 

Enter the viewer directory:
{% highlight sh %} 
cd LiteViewer
{% endhighlight %} 

Start the viewer:
{% highlight sh %} 
java -jar adlite-viewer.jar
{% endhighlight %} 

Open `URL http://localhost:8990/` with your browser. Default user is `admin` 
with password `admin`. You see an empty dashboard.

If you monitor an OSGI runtime with AppDynamics, you have to extend the boot 
delegation parameter of FUSE ESB. See 
<a href="http://litedocs.appdynamics.com/display/ADLite/OSGi+Infrastructure">http://litedocs.appdynamics.com/display/ADLite/OSGi+Infrastructure</a> 
for further explanations. As I use Felix, I have to add `com.singularity.*` to 
the `org.osgi.framework.bootdelegation` property. For easier upgrade of FUSE ESB, 
I do not edit `<FUSE_ESB_HOME>/etc/config.properties` but `<FUSE_ESB_HOME>/etc/custom.properties`. 

It is important that you don\'t forget to add all default values. And of course these values 
may change if you upgrade your FUSE ESB installation.  Finally, I add following line to 
`<FUSE_ESB_HOME>/etc/custom.properties`:   
{% highlight sh %} 
org.osgi.framework.bootdelegation=
    com.singularity.*,
    org.apache.karaf.jaas.boot,
    sun.*,com.sun.*,
    javax.transaction,
    javax.transaction.*,
    org.apache.xalan.processor,
    org.apache.xpath.jaxp,
    org.apache.xml.dtm.ref,
    org.apache.xerces.jaxp.datatype,
    org.apache.xerces.stax,
    org.apache.xerces.parsers,
    org.apache.xerces.jaxp,org.apache.xerces.jaxp.validation,
    org.apache.xerces.dom
{% endhighlight %} 

If your are not sure which OSGI framework you use, you can query your configuration in your 
Karaf console with:
{% highlight sh %} 
shell:info
{% endhighlight %} 

Configure AppDynamics agent for FUSE ESB. E.g., set `$KARAF_OPTS` in your shell:
{% highlight sh %} 
export KARAF_OPTS="$KARAF_OPTS -javaagent:<APP_DYNAMICS_HOME>/javaagent.jar"
{% endhighlight %} 

Or alternatively, add following statement to your start script `<FUSE_ESB_HOME>/bin/karaf`:
{% highlight sh %} 
KARAF_OPTS="$KARAF_OPTS -javaagent:<APP_DYNAMICS_HOME>/javaagent.jar"
{% endhighlight %} 

Start your FUSE ESB (in the same shell where you set `$KARAF_OPTS`):
{% highlight sh %} 
cd <FUSE_ESB_HOME>/bin/fuse
{% endhighlight %} 

Create some traffic on your FUSE ESB, e.g. invoke a web service etc.

Go back to your AppDynamics viewer browser window at `http://localhost:8990/` 
and you should see some business transactions.

<a href="http://1.bp.blogspot.com/-dwT6fA6c6Qc/UbzItTFSMXI/AAAAAAAAAlE/fZax_LgVWl4/s1600/appdynamics_overview.png" style="margin-left: 1em; margin-right: 1em;" target="_blank"><img border="0" src="http://1.bp.blogspot.com/-dwT6fA6c6Qc/UbzItTFSMXI/AAAAAAAAAlE/fZax_LgVWl4/s400/appdynamics_overview.png" width="580" /></a>

Finito.
