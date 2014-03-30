---
layout: post
status: publish
published: true
title: Migrating to Wicket 1.5
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
excerpt: Alexandros Karypidis has published his <a href="http://alexandros-karypidis.blogspot.com/2011/01/migrating-from-wicket-14-to-15.html">experiences
  migrating from 1.4 to 1.5</a>
wordpress_id: 570
wordpress_url: http://wicketinaction.com/?p=570
date: '2011-04-26 14:54:00 +0200'
date_gmt: '2011-04-26 12:54:00 +0200'
categories:
- Wicket
tags: []
comments: []
---
<p>At my $dayjob Wicket 1.4 still rules supreme, but work is under way to <a href="http://twitter.com/#!/dashorst/status/58089114746097664">migrate our apps to 1.5</a>. Our migration experiences should make a nice blog entry, but meanwhile Alexandros Karypidis has published his <a href="http://alexandros-karypidis.blogspot.com/2011/01/migrating-from-wicket-14-to-15.html">experiences migrating from 1.4 to 1.5</a>:</p>
<blockquote><p>1) Maven dependency has changed<br/><br />
2) It's JavaScript (camel-case) not Javascript<br/><br />
3) Header contribution is now implemented in Component and Behavior<br/><br />
4) Template-base CSS and JavaScript (TextTemplateHeaderContributor) must now be contributed to the header<br/><br />
5) Page mounting<br/><br />
6) Mounting of global resources
</p></blockquote>
<p>Not too bad for a big upgrade. Of course, you can always refer to our <a href="https://cwiki.apache.org/WICKET/migration-to-wicket-15.html">1.5 migration guide</a>.</p>
