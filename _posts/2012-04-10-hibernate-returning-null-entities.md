---
layout: post
title: Hibernate returning NULL entities?
date: '2012-04-10T13:04:00.000+02:00'
author: Peter Keller
tags:
- ORM
modified_time: '2013-06-18T21:56:43.819+02:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-3093611572025737001
blogger_orig_url: http://peter-on-java.blogspot.com/2012/04/hibernate-returning-null-entities.html
---

Ever got `NULL` entity entries from Hibernate? I did. 

Having a table `V_MEAS_SITE` with following attributes: 

{% highlight text %} 
MEAS_SITE_ID:     NUMBER(8
INSTALLATION_ID   NUMBER(8
NAME_TX           VARCHAR2(10
...
{% endhighlight %} 

The `V_` in the table name indicates that this is a Oracle view and not a table. 
This name follows our naming conventions. As the name of the view indicates, 
the view is holding all information about measurement sites (I actually work 
for the national weather service). This table is mapped to the following Java class: 

{% highlight java %} 
@Entity(table="V_MEAS_SITE")
public class MeasurementSite {

    @Id
    @Column(name="MEAS_SITE_ID")
    private int measurementSiteId;
    
    @Column(name="INSTALLATION_ID")
    private int installationId;

    @Column(name="NAME_TX")
    private String name;

    // getter/setter are ommited for brevit
    
    @Overrid
    public int hashCode() { 
        final int prime = 31;
        int result = 1;
        return = prime * result + ((getMeasurementSiteId() == null) ? 0 : getMeasurementSiteId().hashCode());
    }

    @Overrid
    public boolean equals(Object obj) { 
        if (this == obj) { 
            return true;
        }
        if (obj == null) { 
            return false;
        }
        if (!(obj instanceof MeasurementSite)) { 
            return false;
        }
        MeasurementSite other = (MeasurementSite)obj;
        if (installationId == null) {
            if (other.getInstallationId() != null) { 
                return false;
            }
        } else if (!getMeasurementSiteId().equals(other.getMeasurementSiteId())) { 
            return false;
        }
        return true;
    }
}
{% endhighlight %} 


Let\'s write an EJB session bean reading some data:

{% highlight java %} 
@Session
public class MeasurementSiteService implements MeasurementSiteLocal {

    @PersistenceContext(unitName="DefaultEntityManager")
    private EntityManager em;

    public List<MeasurementSite> findByName(String name) {
        String sql = "select m from MeasurementSite where m.name like :name";
        return em.createQuery(sql)
            .setParameter("name", name)
            .list();
    }

    // used for testing
    public void setEntityManager(EntityManager em) { 
        this.em = em;
    }

}
{% endhighlight %} 

That\'s easy. OK, let\'s test this mapping with a JUnit test class:

{% highlight java %} 
public class MeasurementSiteServiceTest {

    private EntityManager em = ....; // set up the entity manager  

    @Test
    public void findByName() 
         MeasurementSiteService s = new MeasurementSiteService();
         s.setEntityManager(em);
         List<MeasurementSite> list = s.findByName("Z%");
         for (MeasurementSite m: list) {
             Assert.notNull(m);
         }
    }
}         
{% endhighlight %} 

However, this test fails. There are indeed some NULL entity entries in the list. 
Hibernate must be broken! Of course, Hibernate is not broken. Do you see what went wrong? 

I did after some time of debugging. The view catches content of different tables and is 
therefore de-normalized. The primary key of the `V_MEAS_SITE` view is not `MEAS_SITE_ID` 
but `INSTALLATION_ID`! 

As a view has no explicit contraints there is no primary key constraint neither and therefore 
you have no explicit indications about the \"logic\" in the data. In this case it means that 
there are more than one entry for a given measurement site ID. And that\'s why Hibernate 
returned the `NULL` values. 

Of course it would be nice, if Hibernate had thrown some Exceptions to help 
a careless Java developer\... 

By the way, Oracle DB allows to query the DLL statement:

{% highlight sql %} 
select dbms_metadata.get_ddl('VIEW', 'V_MEAS_SITE') as dll from dua
{% endhighlight %} 
