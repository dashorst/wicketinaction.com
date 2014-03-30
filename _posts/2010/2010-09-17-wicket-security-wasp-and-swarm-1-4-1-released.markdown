---
layout: post
status: publish
published: true
title: 'Wicket Security: WASP and SWARM 1.4.1 released!'
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 505
wordpress_url: http://wicketinaction.com/?p=505
date: '2010-09-17 13:47:17 +0200'
date_gmt: '2010-09-17 11:47:17 +0200'
categories:
- Wicket
tags: []
comments: []
---
<p>The Wicket Security project WASP/SWARM has released a new version: 1.4.1</p>
<p>News worthy changes:</p>
<ul>
<li>Moved code from SwarmStrategy to AbstractSwarmStrategy to allow reuse with different implementations</li>
<li>Logout now uses Session.invalidate() instead of invalidateNow(), to prevent problems with the request logger</li>
<li>Spring example is now based on Spring 3</li>
<li>Wicket dependency upgraded to 1.4.12</li>
</ul>
<p>You can download the release from the Wicket stuff repository:</p>
<ul>
<li><a href="http://wicketstuff.org/maven/repository/org/apache/wicket/wicket-security/">http://wicketstuff.org/maven/repository/org/apache/wicket/wicket-security/</a></li>
</ul>
<p>Or upgrade using the following in your pom:</p>
{% highlight xml %}<dependency>
   <groupId>org.apache.wicket.wicket-security</groupId>
   <artifactId>swarm</artifactId>
   <version>1.4.1</version>
</dependency>
{% endhighlight %}
