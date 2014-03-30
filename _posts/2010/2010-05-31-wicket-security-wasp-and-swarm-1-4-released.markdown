---
layout: post
status: publish
published: true
title: 'Wicket Security: WASP and SWARM 1.4 released!'
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 501
wordpress_url: http://wicketinaction.com/?p=501
date: '2010-05-31 16:12:16 +0200'
date_gmt: '2010-05-31 14:12:16 +0200'
categories:
- Wicket
tags:
- Releases
- security
- swarm
- wasp
comments:
- id: 694
  author: Simon
  author_email: anonymous@yahoo.com
  author_url: ''
  date: '2010-07-08 01:51:26 +0200'
  date_gmt: '2010-07-07 23:51:26 +0200'
  content: I have been using SWARM and WASP for my small business app, very good.
---
<p>We are proud to release Wicket Security 1.4 final.</p>
<p>Wicket Security is an attempt to create an out of the box reusable authenticating and authorization framework for Apache Wicket. It contains several projects which can be used standalone or in conjunction with each other.</p>
<p>After testing the codebase for a while we did not find any issues. </p>
<p>Differences between the 1.4-rc1 release: </p>
<ul>
<li>upgraded dependencies to newest working versions (JUnit 4.x does not work with Spring)</li>
<li>versioned maven plugins to appease the Maven 3 gods.</li>
</ul>
<p>Many thanks go to Olger Warnier for the initial port of Wicket Security to Wicket 1.4.</p>
<p>The release is available from the <a href="http://wicketstuff.org/maven/repository/org/apache/wicket/wicket-security">Wicket Stuff maven repository</a>.</p>
<p>If you already depend on Wicket Security, all you need to do is modify the version of your dependencies in your Maven poms:</p>

{% highlight xml %}
	<repository>
		<id>wicketstuff</id>
		<url>http://wicketstuff.org/maven/repository</url>
		<snapshots>
			<enabled>true</enabled>
		</snapshots>
		<releases>
			<enabled>true</enabled>
		</releases>
	</repository>

	<dependency>
		<groupId>org.apache.wicket.wicket-security</groupId>
		<artifactId>swarm</artifactId>
		<version>1.4</version>
		<scope>compile</scope>
	</dependency>
{% endhighlight %}

<p>Note that with future releases we will move to a new groupId and package name (since <code>org.apache.wicket</code> is reserved for Apache Wicket, and not 3rd party projects).</p>
<p>The future of the Wicket Security project is to remain a standalone project (it will not be adopted by Apache Wicket), and will continue to be maintained by Topicus. If you wish to join please let us know!</p>
<p>Emond &amp; Martijn</p>
