---
layout: post
title: 'Apache Camel: Fetching the source code with Git and building a tagged release'
date: '2014-05-12T22:24:00.001+02:00'
author: Peter Keller
tags:
- Version Control System
- Camel
- Buildtool
modified_time: '2014-05-12T22:24:35.805+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-4343344171490961423
blogger_orig_url: http://peter-on-java.blogspot.com/2014/05/apache-camel-fetching-source-code-with.html
---

Clone the whole repository:

{% highlight sh %}
git clone https://git-wip-us.apache.org/repos/asf/camel.git
{% endhighlight %}    

After the clone, you can list the tags:

{% highlight sh %}
cd camel
git tag -l 
{% endhighlight %}    

For Apache Camel this is:

{% highlight sh %}
...
camel-2.12.0
camel-2.12.1
camel-2.12.2
camel-2.12.3
camel-2.13.0
camel-2.13.1
...
{% endhighlight %}    

Checkout a specific tag:

{% highlight sh %}
git checkout tags/camel-2.13.1
{% endhighlight %}    

Finally, you build the Camel 2.13.1 release:

{% highlight sh %}
mvn clean install
{% endhighlight %}    
