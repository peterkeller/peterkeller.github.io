---
layout: post
title: 'Fun with Oracle DB: ORA-00907'
date: '2012-03-21T08:42:00.030+01:00'
author: Peter Keller
tags:
- Database
modified_time: '2013-06-18T21:54:34.098+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-5147247521570663463
blogger_orig_url: http://peter-on-java.blogspot.com/2012/03/fun-with-oracle-db-ora-00907missing.html
---

Following SQL select statement 

{% highlight sql %} 
select * from A a
where a.id in (select b.id from B b order by b.id)
{% endhighlight %} 

leads to an error message:

{% highlight text %} 
> ORA-00907: missing right parenthesis 
{% endhighlight %} 

Hm. I don\'t see anything missing\... Ok, let\'s remove the order clause, as it is 
not really needed:

{% highlight sql %} 
select * from A a
where a.id in (select b.id from B b)
{% endhighlight %} 
    
No `ORA-00907`. I am not the only one having these troubles, see the blog post by 
<a href="http://oraclequirks.blogspot.com/2008/01/ora-00907-missing-right-parenthesis.html">oraclequirks</a>.
