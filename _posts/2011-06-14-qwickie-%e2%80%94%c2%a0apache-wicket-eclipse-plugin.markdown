---
layout: post
status: publish
published: true
title: QWickie — Apache Wicket Eclipse Plugin
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 585
wordpress_url: http://wicketinaction.com/?p=585
date: '2011-06-14 13:20:46 +0200'
date_gmt: '2011-06-14 11:20:46 +0200'
categories:
- Wicket
tags:
- Wicket
- eclipse
comments:
- id: 1043
  author: Clint
  author_email: checketts@gmail.com
  author_url: http://checkettsweb.com
  date: '2011-06-14 13:53:00 +0200'
  date_gmt: '2011-06-14 11:53:00 +0200'
  content: Do you have a direct link to the site? (Yeah I could google for it, but
    a link is even better)
- id: 1044
  author: dashorst
  author_email: martijn.dashorst@gmail.com
  author_url: http://martijndashorst.com
  date: '2011-06-14 14:09:58 +0200'
  date_gmt: '2011-06-14 12:09:58 +0200'
  content: Fixed the markup... I had the link in the post, but with a wrong tag.
- id: 1045
  author: martin-g
  author_email: martin.grigorov@gmail.com
  author_url: ''
  date: '2011-06-14 14:54:22 +0200'
  date_gmt: '2011-06-14 12:54:22 +0200'
  content: The latest version is actually 0.8.3.
---
<p>Eclipse users have been at a disadvantage for Apache Wicket development when compared to Netbeans or IntelliJ IDEA users. For those IDEs some nice integration plugins exist for web development using Wicket.</p>
<p>I just discovered <a href="http://qwickie.googlecode.com/">QWickie</a>, a plugin for Eclipse, which has the following features:</p>
<ul>
<li>Navigate from java code elements to the corresponding html element via wicket:id</li>
<li>Show the corresponding html fragment from the java code element</li>
<li>Show the wicket:id line in the html file on mouse over</li>
<li>Rename HTML when renaming a wicketized java file (supertype Component)</li>
<li>Find html files in other locations</li>
<li>Jump to properties files</li>
<li>Jump to xml files</li>
<li>Support for other wicket namespaces</li>
<li>Wizard for new wicket pages</li>
<li>Code assist for wicket:id (Ctrl-1) in Wicket Java files. e.g. new Label("&lt;press Ctrl-1&gt;")</li>
</ul>
<p>Be aware that this plugin is still 0.0.4, so use at your own risk!</p>
