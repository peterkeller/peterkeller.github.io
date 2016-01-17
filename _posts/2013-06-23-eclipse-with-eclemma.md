---
layout: post
title: 'Eclipse with Eclemma: java.lang.NoClassDefFoundError: oracle/security/pki/OracleWallet'
date: '2013-06-23T21:08:00.000+02:00'
author: Peter Keller
tags:
- IDE
modified_time: '2013-06-23T21:25:23.001+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-2916617889118098256
blogger_orig_url: http://peter-on-java.blogspot.com/2013/06/eclipse-with-eclemma.html
---

Trying to determine the code coverage of my JUnit 4 tests with EclEmma an `java.lang.NoClassDefFoundError` was thrown. As we all love Java stack traces, here a short excerpt:

{% highlight sh %} 
java.lang.NoClassDefFoundError: oracle/security/pki/OracleWallet
 at java.lang.Class.forName0(Native Method)
 at java.lang.Class.forName(Class.java:169)
 at org.hibernate.connection.DriverManagerConnectionProvider.configure(DriverManagerConnectionProvider.java:57)
 at org.hibernate.connection.ConnectionProviderFactory.newConnectionProvider(ConnectionProviderFactory.java:124)
 at
 ...
{% endhighlight %} 

What does my JUnit Test wants from the `OracleWallet`? The application uses JDBC for the access of the 
Oracle DB, but `OracleWallet` is never directly used in my application. Without Eclemma the tests 
are running successfully. Not nice.

This seems to be a known problem which is fixed in EclEmma 2.1.3, see 
<a href="http://sourceforge.net/p/eclemma/bugs/108/">http://sourceforge.net/p/eclemma/bugs/108/</a>. 

If you don\'t want (or are not allowed\...) to update, then the  workaround is to exclude `oracle.*` 
from the coverage agent in the Code Coverage preferences, 
see <a href="http://www.eclemma.org/userdoc/preferences.html">http://www.eclemma.org/userdoc/preferences.html</a>.