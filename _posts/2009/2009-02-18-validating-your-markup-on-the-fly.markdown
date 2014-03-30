---
layout: post
status: publish
published: true
title: Validating your markup on the fly
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 352
wordpress_url: http://wicketinaction.com/?p=352
date: '2009-02-18 01:14:21 +0100'
date_gmt: '2009-02-17 23:14:21 +0100'
categories:
- Wicket
tags:
- Wicket
- Validator
comments:
- id: 417
  author: Jonathan Locke
  author_email: jonathan.locke@gmail.com
  author_url: ''
  date: '2009-02-18 08:32:42 +0100'
  date_gmt: '2009-02-18 06:32:42 +0100'
  content: Cool idea!
---
<p>One of the most puzzling things about web development is when something works in one browser, but doesn't in another. Often this is the case when your document is not standards compliant. For example you declare that your markup is XHTML strict, but have a structural problem in your markup. Browsers will try to repair your document, but can take different measures to cope with the problem.</p>
<p>The way to avoid such differences is to ensure that at least your documents are up to spec. Running your website manually through the W3C validator is not an option. So I set out to make this a less painful exercise. Searching the internets provided me with the <a href="http://tuckey.org/validation/">Tuckey ValidationFilter</a>, but integrating it with our application was not really working, because of some CSS issues (z-index, fonts, alignment, coloring). Long story short, I'd figure taking up the ValidationFilter and integrating it someway into a little bit more Wicket like fashion. I've implemented it as a IResponseFilter and have it insert the necessary markup into the response at the end of the head and body sections when the original markup was invalid.</p>
<p>When you enable the HtmlValidatorResponseFilter in your project, you see your markup mistakes immediately when developing your application. No longer is standards compliant markup an afterthought, but directly visible the moment you fire up your local server and browser.</p>
<p>The htmlvalidator project contains an example application with valid and invalid markup. You can run it by starting the embedded Jetty server using the Start class in the src/test/java folder and pointing your browser to localhost:8080.</p>
<p>The project is available on my <a href="http://github.com/dashorst/wicket-stuff-markup-validator">github space</a>. The project needs to be build using Maven, but that shouldn't hold you back trying it out. It mostly builds upon the code from the Tuckey guys, so major thanks go to them. I'm hoping to find an Apache License compatible, up to date, markup validator so we can ship this functionality out of the box with Wicket 1.5.</p>
<p>Things I'd like to improve:</p>
<ul>
<li>no more GPL code</li>
<li>use Wicket components instead of a generated JSP for the error page</li>
<li>integrate it into a debug console together with Ajax debug, ehcache stats, hibernate stats, page stats, wicket stats, etc.</li>
</ul>
