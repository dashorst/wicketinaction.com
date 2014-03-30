---
layout: post
status: publish
published: true
title: Wicket/Spring/Hibernate configuration
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
excerpt: Setting up Wicket, Spring and Hibernate to get started with a database oriented
  application is always a chore. With this setup you don&#039;t have to figure it
  out yourself and you&#039;ll get an in-memory database, classpath scanning, JPA
  and Spring enabled Wicket application.
wordpress_id: 385
wordpress_url: http://wicketinaction.com/?p=385
date: '2009-06-05 10:24:02 +0200'
date_gmt: '2009-06-05 08:24:02 +0200'
categories:
- Wicket
tags:
- Wicket
- spring
- hibernate
comments:
- id: 502
  author: Stefan F
  author_email: stf+gravatar@molindo.at
  author_url: http://www.molindo.at/
  date: '2009-06-05 10:54:41 +0200'
  date_gmt: '2009-06-05 08:54:41 +0200'
  content: "Cool stuff, bookmarked! This will certainly come in handy some day ;)\r\n\r\nCould
    somebody make this to a maven archetype? That would be awesome!\r\n\r\nCheers,
    Stefan"
- id: 503
  author: Michael Lange
  author_email: daywalker2000@gmx.net
  author_url: ''
  date: '2009-06-05 16:13:50 +0200'
  date_gmt: '2009-06-05 14:13:50 +0200'
  content: "is the \r\n\r\napplicationClassName\r\ncom.mycompany.WicketApplication\r\n\r\nreally
    necessary ? in my configuration it works without it\r\n\r\nim still working with
    wicket 1.4, is http://cwiki.apache.org/WICKET/spring.html#Spring-AnnotationbasedApproach
    still the way to use spring beans inside page (and other) wicket classes ?"
- id: 548
  author: Antoine
  author_email: antoine.van.wel@gmail.com
  author_url: ''
  date: '2009-07-13 23:47:39 +0200'
  date_gmt: '2009-07-13 21:47:39 +0200'
  content: "Nice, haven't got your example working yet using 1.4-rc6;\r\n\r\n- for
    wicket-spring-annot version doesn't work, seems like there are only versions for
    the 1.3 branch, so setting the version to 1.3.6 instead of ${wicket.version} should
    do the trick\r\n\r\n- something seems to have gone wrong with the interceptor
    bean: \r\n\r\n    \r\n\r\nreplacing the com.mycompany by org.springframework.orm.hibernate3
    does not seem to be enough so guess something else went missing\r\n\r\n\r\nfinally,
    it is not mentioned the first file is applicationContext.xml"
- id: 567
  author: David
  author_email: david@davidwbrown.name
  author_url: http://www.davidwbrown.name
  date: '2009-08-05 00:53:21 +0200'
  date_gmt: '2009-08-04 22:53:21 +0200'
  content: Hello Antoine, I am having similar problems. Did you ever get this to working?
- id: 577
  author: buddy
  author_email: vijay@mxtelecom.com
  author_url: ''
  date: '2009-08-19 00:03:55 +0200'
  date_gmt: '2009-08-18 22:03:55 +0200'
  content: what is the name and path of the first file you created? it's very difficult
    for a newbie to figure it out.
- id: 606
  author: Simon
  author_email: svwpsv@gmail.com
  author_url: ''
  date: '2009-10-18 21:30:40 +0200'
  date_gmt: '2009-10-18 19:30:40 +0200'
  content: Great setup tutorial, tobad it doesn't work! :-(
- id: 663
  author: Alexandros Karypidis
  author_email: akarypid@yahoo.gr
  author_url: http://gr.linkedin.com/in/alexandroskarypidis
  date: '2010-02-10 16:41:34 +0100'
  date_gmt: '2010-02-10 14:41:34 +0100'
  content: "For those of you that want to work with Wicket 1.4.6:\r\n\r\nThere dependency
    on \"wicket-spring-annot\" is no longer required. Simply remove it. This package
    was used only in Wicket 1.3.\r\n\r\nIts purpose was to add annotation support
    due to the fact that Wicket 1.3 could run on JDK 1.4, which did not have annotations.
    Starting with Wicket 1.4, JDK 5 (or later) is required and therefore the annotation
    support is not needed.\r\n\r\nRemove the  on that and it should work with Wicket
    1.4 as well."
- id: 667
  author: Erik
  author_email: erikbalek@yahoo.com
  author_url: ''
  date: '2010-03-26 15:02:21 +0100'
  date_gmt: '2010-03-26 13:02:21 +0100'
  content: "I have been trying this for a while now but I can't get it to work. I
    removed hibernate3-maven-plugin from the pom and copied the above code (with corrections).
    The sessionFactory and the interceptor have a cyclic dependency: \r\norg.springframework.beans.factory.BeanCreationException:
    Error creating bean with name 'sessionFactory' defined in class path resource
    [applicationContext.xml]: Cannot resolve reference to bean 'interceptor' while
    setting bean property 'entityInterceptor'; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException:
    Error creating bean with name 'interceptor' defined in class path resource [applicationContext.xml]:
    Unsatisfied dependency expressed through bean property 'sessionFactory': : Error
    creating bean with name 'sessionFactory': FactoryBean which is currently in creation
    returned null from getObject; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException:
    Error creating bean with name 'sessionFactory': FactoryBean which is currently
    in creation returned null from getObject\r\nI have searched a lot on the Spring
    forum and seen some advice on cyclic dependencies, like BeanPostProcessor, but
    I can't get it to work. Does anyone here have the same experience and have solved
    the problem?"
- id: 668
  author: Joonas
  author_email: joonas.koivunen@gmail.com
  author_url: ''
  date: '2010-04-06 14:09:15 +0200'
  date_gmt: '2010-04-06 12:09:15 +0200'
  content: "@Erik: Do not use an interceptor. Simple as that; you'll be much better
    off using event listeners. If you really have to use the interceptor (which quickly
    becomes a god-like class) then you can always implement ApplicationContextAware
    interface and manually fetch beans from that.\r\n\r\nBtw, sounds a bit strange
    that your interceptor would have to have a sessionFactory reference. Perhaps its
    there by mistake?"
- id: 742
  author: Adam Gibbons
  author_email: adam.s.gibbons@gmail.com
  author_url: http://none
  date: '2010-12-15 13:20:53 +0100'
  date_gmt: '2010-12-15 11:20:53 +0100'
  content: "Hi there,\r\n\r\nI'm having some problems getting this example to work.
    Please see my topic at: http://wicket-users.markmail.org/message/gonb5alcclwpgnod?q=Wicket/Spring/Hibernate+configuration\r\n\r\nIf
    anyone can shed any light on this I would very much appreciate it!"
- id: 858
  author: Peter
  author_email: ptriller@soapwars.de
  author_url: ''
  date: '2011-04-26 16:40:36 +0200'
  date_gmt: '2011-04-26 14:40:36 +0200'
  content: A session in view filter on /* ? that will also make a Hibernate session
    for css and image requests. Not very resource friendly
---
<p>I like annotations and loathe oodles of XML. To get Spring and Hibernate work with those nice <tt>@Transactional</tt>, <tt>@Service</tt> and <tt>@Entity</tt> annotations by scanning your classpath, the following Spring configuration files should do the trick for your default <a href="http://wicket.apache.org/quickstart.html">Wicket Quickstart</a> project (provided you add the necessary Spring and Hibernate dependencies to your setup). Figuring this out took me an evening (especially getting the scanners and <tt>@Transactional</tt> support up and running), so here"s my setup such that you don"t have to spend the evening yourself:<br />
<a id="more"></a><a id="more-385"></a></p>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans default-autowire="autodetect"
    xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd
           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-2.5.xsd">

    <bean id="wicketApplication" class="com.mycompany.WicketApplication" />

    <bean id="placeholderConfigurer"
        class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="ignoreUnresolvablePlaceholders" value="false" />
        <property name="systemPropertiesModeName" value="SYSTEM_PROPERTIES_MODE_OVERRIDE" />
        <property name="ignoreResourceNotFound" value="false" />
        <property name="locations">
            <list>
                <value>classpath*:/application.properties</value>
            </list>
        </property>
    </bean>

    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="driverClassName">
            <value>${jdbc.driver}</value>
        </property>
        <property name="url">
            <value>${jdbc.url}</value>
        </property>
        <property name="username">
            <value>${jdbc.username}</value>
        </property>
        <property name="password">
            <value>${jdbc.password}</value>
        </property>
    </bean>

    <tx:annotation-driven transaction-manager="txManager" />
    
    <!-- setup transaction manager  -->
    <bean id="txManager"
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory">
            <ref bean="sessionFactory" />
        </property>
    </bean>

    <bean id="interceptor" class="com.mycompany.hibernate.HibernateInterceptor">
    </bean>

    <!-- hibernate session factory -->
    <bean id="sessionFactory"
        class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="hibernateProperties">
            <props>
                <prop key="hibernate.hbm2ddl.auto">create</prop>
                <prop key="hibernate.dialect">${hibernate.dialect}</prop>
                <prop key="hibernate.connection.pool_size">5</prop>
                <prop key="hibernate.current_session_context_class">thread</prop>
                <prop key="hibernate.show_sql">true</prop>
                <prop key="hibernate.cglib.use_reflection_optimizer">true</prop>
                <prop key="hibernate.cache.provider_class">org.hibernate.cache.EhCacheProvider</prop>
                <prop key="hibernate.hibernate.cache.use_query_cache">true</prop>
            </props>
        </property>
        <property name="entityInterceptor">
            <ref bean="interceptor" />
        </property>
        <property name="packagesToScan">
            <list>
                <value>com.mycompany.entities</value>
            </list>
        </property>
    </bean>
    <context:component-scan base-package="com.mycompany" />
</beans>
{% endhighlight %}

<p>This setup requires one additional file&mdash;<tt>application.properties</tt>:</p>

{% highlight properties %}
jdbc.driver=org.hsqldb.jdbcDriver
jdbc.url=jdbc:hsqldb:mem:phonebook
jdbc.username=sa
jdbc.password=
hibernate.dialect=org.hibernate.dialect.HSQLDialect
{% endhighlight %}

<p>This will configure Spring and Hibernate to use an in memory HSQLDB database. You"ll need to download the HSQLDB jar for this as well. Or you can configure this to use mysql, oracle, sqlserver or postgresql. Put these files in <tt>src/main/resources</tt>.</p>
<p>I have to add the following dependencies to my quickstart pom to get HSQLDB, Hibernate and Spring working:</p>

{% highlight xml %}
<dependency>
    <groupId>org.apache.wicket</groupId>
    <artifactId>wicket-spring-annot</artifactId>
    <version>${wicket.version}</version>
</dependency>

<!-- HIBERNATE DEPENDENCIES -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate</artifactId>
    <version>3.2.6.ga</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>ejb3-persistence</artifactId>
    <version>3.3.2.Beta1</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-annotations</artifactId>
    <version>3.4.0.GA</version>
</dependency>
<dependency>
    <groupId>commons-dbcp</groupId>
    <artifactId>commons-dbcp</artifactId>
    <version>1.2.2</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring</artifactId>
    <version>2.5.6</version>
    <scope>compile</scope>
</dependency>

<!-- DATABASE DEPENDENCIES - HSQLDB -->
<dependency>
    <groupId>hsqldb</groupId>
    <artifactId>hsqldb</artifactId>
    <version>1.8.0.7</version>
</dependency>
{% endhighlight %}

<p>You"ll also have to configure the <tt>web.xml</tt> file to enable Spring and Spring"s <tt>OpenSessionInView</tt> filter.:</p>

{% highlight xml %}
<?xml version="1.0" encoding="ISO-8859-1"?>
<web-app xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
    version="2.4">

    <display-name>wicket-spring-hibernate</display-name>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <filter>
        <filter-name>opensessioninview</filter-name>
        <filter-class>org.springframework.orm.hibernate3.support.OpenSessionInViewFilter</filter-class>
    </filter>

    <filter>
        <filter-name>wicket-spring-hibernate</filter-name>
        <filter-class>org.apache.wicket.protocol.http.WicketFilter</filter-class>
        <init-param>
            <param-name>applicationFactoryClassName</param-name>
            <param-value>org.apache.wicket.spring.SpringWebApplicationFactory</param-value>
        </init-param>
        <init-param>
            <param-name>applicationClassName</param-name>
            <param-value>com.mycompany.WicketApplication</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>opensessioninview</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <filter-mapping>
        <filter-name>wicket-spring-hibernate</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
{% endhighlight %}

<p>The world is your oister!</p>
