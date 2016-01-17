---
layout: post
title: 'Gradle: From zero to war'
date: '2016-01-15T09:56:00.003+01:00'
author: Peter Keller
tags:
- Buildtool
modified_time: '2016-01-15T16:17:20.068+01:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-5847681905042169486
blogger_orig_url: http://peter-on-java.blogspot.com/2016/01/gradle-from-zero-to-war.html
---

The well known `gradle init` task is an easy way to create a Java project: 

{% highlight sh %}
> gradle init --type java-library
{% endhighlight %}   
    
However, there is no support for initializing a web application. Alternatively, you may use the cool gradle-templates of townsfolk: [https://github.com/townsfolk/gradle-templates](https://github.com/townsfolk/gradle-templates).  

How this may be done, is described below.

## Step 0: Create master gradle script

Create a master `build.gradle` script. This may be done anywhere. However, I put it in the project parent directory where all my projects are found:

{% highlight text %}
buildscript {
    repositories {
        maven {
            url 'http://dl.bintray.com/cjstehno/public'
        }
    }
    dependencies {
        classpath 'gradle-templates:gradle-templates:1.4.1'
    }
}
apply plugin:'templates'
{% endhighlight %}   

List the tasks:

{% highlight sh %}
> gradle tasks
[...]
Template tasks
--------------
createGradlePlugin - Creates a new Gradle Plugin project in a new directory named after your project.
createGroovyClass - Creates a new Groovy class in the current project.
createGroovyProject - Creates a new Gradle Groovy project in a new directory named after your project.
createJavaClass - Creates a new Java class in the current project.
createJavaProject - Creates a new Gradle Java project in a new directory named after your project.
createScalaClass - Creates a new Scala class in the current project.
createScalaObject - Creates a new Scala object in the current project.
createScalaProject - Creates a new Gradle Scala project in a new directory named after your project.
createWebappProject - Creates a new Gradle Webapp project in a new directory named after your project.
exportAllTemplates - Exports all the default template files into the current directory.
exportGroovyTemplates - Exports the default groovy template files into the current directory.
exportJavaTemplates - Exports the default java template files into the current directory.
exportPluginTemplates - Exports the default plugin template files into the current directory.
exportScalaTemplates - Exports the default scala template files into the current directory.
exportWebappTemplates - Exports the default webapp template files into the current directory.
initGradlePlugin - Initializes a new Gradle Plugin project in the current directory.
initGroovyProject - Initializes a new Gradle Groovy project in the current directory.
initJavaProject - Initializes a new Gradle Java project in the current directory.
initScalaProject - Initializes a new Gradle Scala project in the current directory.
initWebappProject - Initializes a new Gradle Webapp project in the current directory.
[...]
{% endhighlight %}
    
As you can see, there are many different templates you may use. For our purpose, the `createWebappProject` task seems to be interesting. 

## Step 1: Create the web project

Use the `createWebappProject` task for creating a new fresh web project. Invoke the task and answer some questions: 

{% highlight sh %}
> gradle createWebappProject
:createWebappProject

templates> Project Name:  (WAITING FOR INPUT BELOW)
> Building 0% > :createWebappProjecttestweb

templates> Project Parent Directory: [/Development/java/projects/projects-test]  (WAITING FOR INPUT BELOW)
> Building 0% > :createWebappProject

templates> Use Jetty Plugin? (Y|n) [n]  (WAITING FOR INPUT BELOW)
> Building 0% > :createWebappProjectY

templates> Group: [testweb]  (WAITING FOR INPUT BELOW)
> Building 0% > :createWebappProject

templates> Version: [0.1]  (WAITING FOR INPUT BELOW)
> Building 0% > :createWebappProject

BUILD SUCCESSFUL

Total time: 25.877 secs
{% endhighlight %}

Check the generated files and directories:

{% highlight sh %}
> cd testweb/
> find .
.
./build.gradle
./gradle.properties
./LICENSE.txt
./src
./src/main
./src/main/java
./src/main/resources
./src/main/webapp
./src/main/webapp/WEB-INF
./src/main/webapp/WEB-INF/web.xml
./src/test
./src/test/java
./src/test/resources
{% endhighlight %}
 
 List the tasks: 

 {% highlight sh %}
> gradle tasks
Build tasks
-----------
[...]
war - Generates a war archive with all the compiled classes, the web-app content and the libraries.
[...]

Web application tasks
---------------------
jettyRun - Uses your files as and where they are and deploys them to Jetty.
jettyRunWar - Assembles the webapp into a war and deploys it to Jetty.
jettyStop - Stops Jetty.
[...]
{% endhighlight %}
 
## Step 2: Create your first page and run it 
 
Create the landing page:

{% highlight sh %}
> echo "Hello" > src/main/webapp/index.html 
{% endhighlight %}
 
And finally, run the web application in Jetty: 

{% highlight sh %} 
> gradle jettyRun
Starting a new Gradle Daemon for this build (subsequent builds will be faster).
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
> Building 75% > :jettyRun > Running at http://localhost:8080/testweb
 {% endhighlight %}
 
Open your browser [http://localhost:8080/testweb/](http://localhost:8080/testweb/) and you should see the welcome message. 

## Step 3: Create war
{% highlight sh %}
> gradle clean war
{% endhighlight %}
    
Check if the web archive is generated:

{% highlight sh %}
> find build/libs/
build/libs/
build/libs//testweb-0.1.war
{% endhighlight %}