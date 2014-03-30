---
layout: post
status: publish
published: true
title: Wicket 1.4.1 supports Ajax file uploads
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 457
wordpress_url: http://wicketinaction.com/?p=457
date: '2009-08-20 23:36:55 +0200'
date_gmt: '2009-08-20 21:36:55 +0200'
categories:
- Releases
- Wicket
tags:
- Releases
- Ajax
- File upload
comments:
- id: 580
  author: Apache Wicket 1.4.1 Released with support for AJAX-based file uploads
  author_email: ''
  author_url: http://wicketbyexample.com/apache-wicket-1-4-1-released-supports-ajax-file-uploads/
  date: '2009-08-21 01:00:58 +0200'
  date_gmt: '2009-08-20 23:00:58 +0200'
  content: '[...] Read more about this release [...]'
- id: 582
  author: Wicket 1.4.1 supports Ajax file uploads | Wicket in Action &#171; codesmell.org
  author_email: ''
  author_url: http://www.codesmell.org/blog/2009/08/wicket-141-supports-ajax-file-uploads-wicket-in-action/
  date: '2009-08-21 09:26:25 +0200'
  date_gmt: '2009-08-21 07:26:25 +0200'
  content: '[...] Wicket 1.4.1 supports Ajax file uploads | Wicket in Action. The
    Apache Wicket project is proud to announce the first maintenance release: Apache
    Wicket 1.4.1. [...]'
---
<p>The Apache Wicket project is proud to announce the first maintenance release: Apache Wicket 1.4.1. The most notable change in this release is the transparent support for multipart form submissions via Ajax. Wicket is now smart enough to submit a form using a hidden iframe rather then the standard <tt>XMLHttpRequest</tt> if the form contains file upload fields.</p>
<p>You can download the release here:<br />
<a href="http://www.apache.org/dyn/closer.cgi/wicket/1.4.1">http://www.apache.org/dyn/closer.cgi/wicket/1.4.1</a></p>
<p>Or use this in your Maven pom"s to upgrade to the new version:</p>

{% highlight xml %}
<dependency>
    <groupId>org.apache.wicket</groupId>
</dependency>
{% endhighlight %}

<p>We thank you for your patience and support.</p>
<p>The Wicket Team</p>
