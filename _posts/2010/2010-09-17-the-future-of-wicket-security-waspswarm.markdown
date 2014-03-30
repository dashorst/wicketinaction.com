---
layout: post
status: publish
published: true
title: 'The future of Wicket Security: WASP/SWARM'
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 511
wordpress_url: http://wicketinaction.com/?p=511
date: '2010-09-17 13:49:37 +0200'
date_gmt: '2010-09-17 11:49:37 +0200'
categories:
- Wicket
tags: []
comments:
- id: 697
  author: me
  author_email: me@example.org
  author_url: ''
  date: '2010-09-18 18:51:48 +0200'
  date_gmt: '2010-09-18 16:51:48 +0200'
  content: Just name it Wicket Security.
- id: 701
  author: me
  author_email: me@example.org
  author_url: ''
  date: '2010-09-20 20:27:03 +0200'
  date_gmt: '2010-09-20 18:27:03 +0200'
  content: If I had to choose, then I would prefer Wicket Keeper.
- id: 705
  author: meo
  author_email: vojta.krasa@gmail.com
  author_url: ''
  date: '2010-09-22 22:32:04 +0200'
  date_gmt: '2010-09-22 20:32:04 +0200'
  content: +1 for Wicket Security
- id: 724
  author: dan
  author_email: daniel.teske@web.de
  author_url: ''
  date: '2010-10-06 16:49:14 +0200'
  date_gmt: '2010-10-06 14:49:14 +0200'
  content: '"Wicket Security" would be the best choice.'
- id: 740
  author: Alex
  author_email: a@a.ru
  author_url: ''
  date: '2010-11-02 15:00:25 +0100'
  date_gmt: '2010-11-02 13:00:25 +0100'
  content: It feels better to make any usefull documentation for the project rather
    than seeking for a name (
- id: 749
  author: Vin
  author_email: vinays.work@gmail.com
  author_url: ''
  date: '2010-12-27 17:20:09 +0100'
  date_gmt: '2010-12-27 15:20:09 +0100'
  content: +1 wicket keeper
---
<p>As Wicket Security will not be adopted into core, we'll be changing<br />
the package name and project name going forward. We're still not sure<br />
about the final name, but these two are the runners up:</p>
<ul>
<li>Chitin</li>
<li>Wicket Keeper</li>
</ul>
<p>Both are nice names, and both have their pros and cons. Let us know<br />
which one you prefer.</p>
<p>Furthermore we'll be adding new annotations such that you'll be able<br />
to authorize your pages using a Java class (for the principal) and an<br />
annotation on your page to specify which principals are required. This<br />
will eliminate the need for the policy files.</p>
<p>These changes will come in the first 1.5 milestone release.</p>
<h3>Future milestones</h3>
<ul>
<li>Support for Wicket 1.5</li>
<li>A new home</li>
<li>Deployment to maven central instead of wicketstuff repo</li>
</ul>
<p>We expect to release the first milestone in a week or so. The final 1.5 release will occur some time after Wicket 1.5 has been finalized.</p>
