---
layout: post
title: JBoss Wildfly with Apache Camel
date: '2014-05-02T15:56:00.000+02:00'
author: Peter Keller
tags:
- javaee
- camel
modified_time: '2016-01-15T10:00:15.109+01:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-6333110973531361191
blogger_orig_url: http://peter-on-java.blogspot.com/2014/05/jboss-wildfly-with-apache-camel.html
---

## Wildfly Camel Integration

This sounds interesting: Apache Camel integration with the WildFly Application Server:

 - Home page: [https://docs.jboss.org/author/display/wfcam/Home](https://docs.jboss.org/author/display/wfcam/Home)
 - User guide: [https://docs.jboss.org/author/display/wfcam/User+Guide](https://docs.jboss.org/author/display/wfcam/User+Guide)
 - Installer: [http://sourceforge.net/projects/jboss/files/WildFly-Camel/](http://sourceforge.net/projects/jboss/files/WildFly-Camel/)

There are two ways to deploy a Camel Context to WildFly

 1. As a single XMl file with a predefined `-camel-context.xml` file suffix
 2. As part of another WildFly supported deployment as `META-INF/jboss-camel-context.xml` file 

## JBoss Modules

Alternatively, see [here](http://sourcevirtues.wordpress.com/2013/11/25/add-apache-camel-and-spring-as-jboss-module-in-wildfly/) 
how Camel dependencies can be added to JBoss Wildfly using JBoss modules. 
