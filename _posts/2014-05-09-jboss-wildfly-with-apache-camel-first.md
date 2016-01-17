---
layout: post
title: 'JBoss Wildfly with Apache Camel: Unsuccessful test'
date: '2014-05-09T19:37:00.001+02:00'
author: Peter Keller
tags:
- Java EE
- Camel
modified_time: '2016-01-15T10:01:10.052+01:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-4124914548937241944
blogger_orig_url: http://peter-on-java.blogspot.com/2014/05/jboss-wildfly-with-apache-camel-first.html
---


Today, I tried to setup the Wildfly Camel integration. Unfortunately, I was not successful. 
Please see what I have tried.

For my tests I used Wildfly 8.0.0.Final and the Wildfly Camel integration installer 1.0.0.Alpha1.

## Step 1: Download

Download the Wildfly Camel installer from [http://sourceforge.net/projects/jboss/files/WildFly-Camel/1.0.0.Alpha1/](http://sourceforge.net/projects/jboss/files/WildFly-Camel/1.0.0.Alpha1/)

This will download the installer file `wildfly-camel-installer-1.0.0.Alpha1.jar`

## Step 2: Install

Start the installer:

{% highlight sh %}
> cd wildfly-8.0.0.Final/
> java -jar wildfly-camel-install-1.0.0.Alpha1.jar 
{% endhighlight %}    

This will install among others all necessary jars in the modules directory and the standalone camel 
configuration file `standalone/configuration/standalone-camel.xml`.

## Step 3: Running

Start the server process:

{% highlight sh %}
>  cd bin
> ./standalone.sh -c standalone-camel.xml
{% endhighlight %}    

 This produces following error:

{% highlight sh %}
[521]% ./standalone.sh -c standalone-camel.xml
=========================================================================

  JBoss Bootstrap Environment

  JBOSS_HOME: /Developer/Workspace/Java/containers/wildfly-8.0.0.Final

  JAVA: /Library/Java/JavaVirtualMachines/jdk1.7.0_40.jdk/Contents/Home/bin/java

  JAVA_OPTS:  -server -XX:+UseCompressedOops -Xms64m -Xmx512m -XX:MaxPermSize=256m -Djava.net.preferIPv4Stack=true -Djboss.modules.system.pkgs=org.jboss.byteman -Djava.awt.headless=true

=========================================================================

18:24:00,635 INFO  [org.jboss.modules] (main) JBoss Modules version 1.3.0.Final
18:24:02,615 INFO  [org.jboss.msc] (main) JBoss MSC version 1.2.0.Final
18:24:02,950 INFO  [org.jboss.as] (MSC service thread 1-7) JBAS015899: WildFly 8.0.0.Final "WildFly" starting
18:24:05,915 ERROR [org.jboss.as.server] (Controller Boot Thread) JBAS015956: Caught exception during boot: org.jboss.as.controller.persistence.ConfigurationPersistenceException: JBAS014676: Failed to parse configuration
    at org.jboss.as.controller.persistence.XmlConfigurationPersister.load(XmlConfigurationPersister.java:112) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.server.ServerService.boot(ServerService.java:331) [wildfly-server-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.controller.AbstractControllerService$1.run(AbstractControllerService.java:256) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at java.lang.Thread.run(Thread.java:724) [rt.jar:1.7.0_40]
Caused by: javax.xml.stream.XMLStreamException: ParseError at [row,col]:[107,21]
Message: JBAS014788: Unexpected attribute 'host' encountered
    at org.jboss.as.controller.parsing.ParseUtils.unexpectedAttribute(ParseUtils.java:104) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.messaging.MessagingSubsystemParser.processConnectors(MessagingSubsystemParser.java:1102)
    at org.jboss.as.messaging.MessagingSubsystemParser.processHornetQServer(MessagingSubsystemParser.java:227)
    at org.jboss.as.messaging.Messaging13SubsystemParser.processHornetQServers(Messaging13SubsystemParser.java:212)
    at org.jboss.as.messaging.MessagingSubsystemParser.readElement(MessagingSubsystemParser.java:134)
    at org.jboss.as.messaging.MessagingSubsystemParser.readElement(MessagingSubsystemParser.java:93)
    at org.jboss.staxmapper.XMLMapperImpl.processNested(XMLMapperImpl.java:110) [staxmapper-1.1.0.Final.jar:1.1.0.Final]
    at org.jboss.staxmapper.XMLExtendedStreamReaderImpl.handleAny(XMLExtendedStreamReaderImpl.java:69) [staxmapper-1.1.0.Final.jar:1.1.0.Final]
    at org.jboss.as.server.parsing.StandaloneXml.parseServerProfile(StandaloneXml.java:1131) [wildfly-server-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.server.parsing.StandaloneXml.readServerElement_1_4(StandaloneXml.java:458) [wildfly-server-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.server.parsing.StandaloneXml.readElement(StandaloneXml.java:145) [wildfly-server-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.server.parsing.StandaloneXml.readElement(StandaloneXml.java:107) [wildfly-server-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.staxmapper.XMLMapperImpl.processNested(XMLMapperImpl.java:110) [staxmapper-1.1.0.Final.jar:1.1.0.Final]
    at org.jboss.staxmapper.XMLMapperImpl.parseDocument(XMLMapperImpl.java:69) [staxmapper-1.1.0.Final.jar:1.1.0.Final]
    at org.jboss.as.controller.persistence.XmlConfigurationPersister.load(XmlConfigurationPersister.java:104) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    ... 3 more

18:24:05,920 FATAL [org.jboss.as.server] (Controller Boot Thread) JBAS015957: Server boot has failed in an unrecoverable manner; exiting. See previous messages for details.
18:24:05,969 INFO  [org.jboss.as] (MSC service thread 1-7) JBAS015950: WildFly 8.0.0.Final "WildFly" stopped in 4ms
{% endhighlight %}    

That\'s too bad.  Obviously, the XML configuration file `standalone-camel.xml` created by the installer 
meets not the schema expected by the final Wildfly 8.0.0 version. It turns out that the Netty 
connector definition is mal-configured.

## Step 4: Extending standalone.xml

Try to fix the configuration starting with the default `standalone.xml` file. Adding Camel extension:

{% highlight xml %}
<extension module="org.wildfly.camel" />
{% endhighlight %}    

Adding a subsystem with simple Camel context:

{% highlight xml %}
<subsystem xmlns="urn:jboss:domain:camel:1.0">
    <camelContext id="system-context-1">
         &lt;route&gt;
             &lt;from uri="direct:start"/&gt;
             &lt;transform&gt;
                 &lt;simple&gt;Hello #{body}&lt;/simple&gt;
             &lt;/transform&gt;
         &lt;/route&gt;      
    </camelContext>
 </subsystem>
{% endhighlight %}    

Starting Wildfly now leads to following error message in the console:

{% highlight sh %}
19:27:30,306 ERROR [org.jboss.as.controller.management-operation] (ServerService Thread Pool -- 28) JBAS014612: Operation ("add") failed - address: ([("subsystem" => "camel")]): java.lang.NoSuchMethodError: org.jboss.as.naming.service.BinderService.getManagedObjectInjector()Lorg/jboss/msc/inject/Injector;
    at org.wildfly.camel.service.CamelContextFactoryBindingService.addService(CamelContextFactoryBindingService.java:66)
    at org.wildfly.camel.parser.CamelSubsystemAdd$1.execute(CamelSubsystemAdd.java:91)
    at org.jboss.as.controller.AbstractOperationContext.executeStep(AbstractOperationContext.java:591) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.controller.AbstractOperationContext.doCompleteStep(AbstractOperationContext.java:469) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.controller.AbstractOperationContext.completeStepInternal(AbstractOperationContext.java:273) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.controller.AbstractOperationContext.executeOperation(AbstractOperationContext.java:268) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at org.jboss.as.controller.ParallelBootOperationStepHandler$ParallelBootTask.run(ParallelBootOperationStepHandler.java:343) [wildfly-controller-8.0.0.Final.jar:8.0.0.Final]
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145) [rt.jar:1.7.0_40]
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615) [rt.jar:1.7.0_40]
    at java.lang.Thread.run(Thread.java:724) [rt.jar:1.7.0_40]
    at org.jboss.threads.JBossThread.run(JBossThread.java:122) [jboss-threads-2.1.1.Final.jar:2.1.1.Final]
{% endhighlight %}    

## Step 5: Giving up\...
The Wildfly Camel integration sounds interesting. However, it seems not to be ready for use. 
To be continued\...