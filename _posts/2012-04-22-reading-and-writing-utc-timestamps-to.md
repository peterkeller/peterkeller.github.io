---
layout: post
title: Reading and Writing UTC Timestamps to DB with Hibernate
date: '2012-04-22T22:46:00.001+02:00'
author: Peter Keller
tags:
- ORM
modified_time: '2013-06-18T21:56:31.691+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-6980480641435646536
blogger_orig_url: http://peter-on-java.blogspot.com/2012/04/reading-and-writing-utc-timestamps-to.html
---

Reading and writing UTC timestamps to a database when the default timezone may change. E.g., this might happen in an application server if an application running in the same JRE changes the default timezone as follows:    

{% highlight java %} 
TimeZone.setDefault(TimeZone.getTimeZone("Europe/Zurich")); 
{% endhighlight %} 

The easiest solution to solve this problem I know is to create an own mapping type extending the standard Hibernate timestamp type `org.hibernate.type.TimestampType`:  
{% highlight java %} 
package ch.meteoswiss.commons.hibernate;

import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.Calendar;
import java.util.SimpleTimeZone;

/**
 * <tt>Timestamp</tt>: A type that maps an SQL TIMESTAMP to a Java
 * java.util.Date or java.sql.Timestamp using UTC time zone.
 */
public class UTCTimestampType extends org.hibernate.type.TimestampType {
     
    @Override
    public Object get(ResultSet rs, String name) throws SQLException {
        return rs.getTimestamp(name, createUTCCalendar());
    } 

   /**
    * Creates UTC calendar. DO NOT USE a static calendar instance.
    * This may lead to concurrency problems with (at least) the Oracle DB
    * driver!
    * @return Calendar with UTC time zone.
    */
    private static Calendar createUTCCalendar() {
        final Calendar c = Calendar.getInstance();
        c.setTimeZone(new SimpleTimeZone(0, "UTC"));
        return c;
    }

    @Override
    public void set(PreparedStatement st, Object value, int index) 
        throws SQLException {
            
        Timestamp ts;
        if (value instanceof Timestamp) {
            ts = (Timestamp) value;
        } else {
            ts = new Timestamp(((java.util.Date)value).getTime());
        }
        st.setTimestamp(index, ts, createUTCCalendar());
    }

}
{% endhighlight %} 

Use it as follows with Java annotations (you could use the type class also 
in a XML configuration file):  

{% highlight java %} 
import java.util.Date;

import org.hibernate.annotations.Type;
@Entity
@Table(name="...")
public class Data {
   
    private Date receptionTimeDt;

    @Type(type="ch.meteoswiss.commons.hibernate.UTCTimestampType")
    @Column(name="RECEPTION_TIME_DT", nullable=false)
    public Date getReceptionTimeDt() {
            return receptionTimeDt;
    }
}    
{% endhighlight %} 

Of course, this mapping class only works with Hibernate and is not standard JPA. 
