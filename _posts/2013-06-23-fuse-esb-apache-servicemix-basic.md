---
layout: post
title: FUSE ESB / Apache ServiceMix Basic Authentication
date: '2013-06-23T22:08:00.000+02:00'
author: Peter Keller
tags:
- ESB
modified_time: '2013-07-05T10:49:58.165+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-6784759452137712651
blogger_orig_url: http://peter-on-java.blogspot.com/2013/06/fuse-esb-apache-servicemix-basic.html
---

Find below a guide to setup up Basic Authentication for a Restful service running in a JBoss FUSE ESB 6.0 / Apache ServiceMix OSGI runtime. The service itself is not special. The notable configuration is found in `blueprint.xml` and in `pom.xml`.

**Important note: this setup only works for JBoss FUSE ESB 6.0 or newer but not for FUSE ESB 7.1.0 or older!**

The Restful service implementation `CustomerService.java`:
 
{% highlight java %} 
package ch.keller.restws.server;

import java.util.Date;

import javax.annotation.Resource;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.WebApplicationException;
import javax.ws.rs.core.Context;
import javax.ws.rs.core.Request;
import javax.ws.rs.core.SecurityContext;
import javax.ws.rs.core.UriInfo;

import org.apache.cxf.jaxrs.ext.MessageContext;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Path("/customers")
public class CustomerService {
 
 private static final Logger LOG = LoggerFactory.getLogger(CustomerService.class);

 @Resource
 private MessageContext jaxrsContext;
 
 @GET
 @Path("/")
 public String listAll() {
  isUserInRole();
  return new Date()+": Yess!! "+jaxrsContext.getSecurityContext().getUserPrincipal();
 }

 private void isUserInRole() throws WebApplicationException {
  LOG.info("user = " + jaxrsContext.getSecurityContext().getUserPrincipal());
 }

}
{% endhighlight %} 
 
The associated `blueprint.xml` configuration:

{% highlight xml %} 
<blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
 xmlns:jaxrs="http://cxf.apache.org/blueprint/jaxrs" 
 xmlns:jaas="http://karaf.apache.org/xmlns/jaas/v1.0.0"
 xmlns:ext="http://aries.apache.org/blueprint/xmlns/blueprint-ext/v1.0.0"
 xsi:schemaLocation="
  http://www.osgi.org/xmlns/blueprint/v1.0.0 
    http://www.osgi.org/xmlns/blueprint/v1.0.0/blueprint.xsd
  http://cxf.apache.org/blueprint/jaxrs 
    http://cxf.apache.org/schemas/blueprint/jaxrs.xsd
  http://karaf.apache.org/xmlns/jaas/v1.0.0 
    http://karaf.apache.org/xmlns/jaas/v1.0.0
  http://aries.apache.org/blueprint/xmlns/blueprint-ext/v1.0.0 
    http://aries.apache.org/schemas/blueprint-ext/blueprint-ext.xsd">
      
  <!-- Bean to allow the $[karaf.base] property to be correctly resolved -->
  <ext:property-placeholder placeholder-prefix="$[" placeholder-suffix="]"/>
   
  <jaxrs:server id="customerService" address="/crm" >
    <jaxrs:serviceBeans>
      <ref component-id="customerSvc"/>
    </jaxrs:serviceBeans>
    <jaxrs:providers>
      <ref component-id="authenticationFilter"/>
    </jaxrs:providers>
  </jaxrs:server>
    
  <bean id="customerSvc" class="ch.keller.restws.server.CustomerService"/>
  <bean id="authenticationFilter" 
    class="org.apache.cxf.jaxrs.security.JAASAuthenticationFilter" >            
      <property name="contextName" value="karaf"/>
  </bean>
    
  <jaas:config name="karaf">
    <jaas:module flags="required"
      className="org.apache.karaf.jaas.modules.properties.PropertiesLoginModule">
        users = $[karaf.base]/etc/users.properties
    </jaas:module>
  </jaas:config><br /> 
    
  <!-- Don't forget to expose the BackingEngine as an OSGi service. -->
  <service interface="org.apache.karaf.jaas.modules.BackingEngineFactory">
    <bean class="org.apache.karaf.jaas.modules.properties.PropertiesBackingEngineFactory"/>
  </service>
</blueprint>
{% endhighlight %} 


The format of the properties in `users.properties` is as follows, with each line defining a user, its password and associated roles:  

{% highlight ini %} 
user=password[,role][,role]...
{% endhighlight %} 

And finally, the Maven `pom.xml` build script:  

{% highlight xml %} 
<project xmlns="http://maven.apache.org/POM/4.0.0" 
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="
   http://maven.apache.org/POM/4.0.0 
     http://maven.apache.org/xsd/maven-4.0.0.xsd">
 
  <modelVersion>4.0.0</modelVersion>
  <groupId>ch.keller</groupId>
  <artifactId>restws</artifactId><br /> 
  <version>0.0.1-SNAPSHOT</version>
  <packaging>bundle</packaging><br />
  <properties>
    <cxf-version>2.6.8</cxf-version>
    <felix-version>2.3.5</felix-version>
  </properties>
  <dependencies>
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-bundle</artifactId>
      <scope>provided</scope>
      <version>${cxf-version}</version>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.felix</groupId>
        <artifactId>maven-bundle-plugin</artifactId>
        <extensions>true</extensions>
        <version>${felix-version}</version>
        <configuration>
          <instructions>
            <Bundle-SymbolicName>${project.artifactId}</Bundle-SymbolicName>
            <Bundle-Description>${project.description}</Bundle-Description>
            <Import-Package>
                org.apache.karaf.jaas.config,
                org.apache.karaf.jaas.boot.principal,
                org.eclipse.jetty.plus.jaas,
                org.apache.karaf.jaas.boot,
                *
            </Import-Package>
            <Export-Package>ch.keller.restws.server</Export-Package>
          </instructions>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
{% endhighlight %} 

Especially important is the `import-package` section that guarantees that no `java.lang.ClassNotFoundException` is thrown during runtime:  

{% highlight xml %} 
<Import-Package>
  org.apache.karaf.jaas.config,
  org.apache.karaf.jaas.boot.principal,
  org.eclipse.jetty.plus.jaas,
  org.apache.karaf.jaas.boot,
</Import-Package>
{% endhighlight %} 
