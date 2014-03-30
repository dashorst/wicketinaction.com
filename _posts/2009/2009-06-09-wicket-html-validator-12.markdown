---
layout: post
status: publish
published: true
title: Wicket HTML validator 1.2
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 417
wordpress_url: http://wicketinaction.com/?p=417
date: '2009-06-09 11:40:27 +0200'
date_gmt: '2009-06-09 09:40:27 +0200'
categories:
- Wicket
tags:
- wicketstuff
- htmlvalidator
comments:
- id: 558
  author: Wicket HTML validator &laquo; Paul Szulc&#8217;s Blog
  author_email: ''
  author_url: http://paulszulc.wordpress.com/2009/07/30/wicket-html-validator/
  date: '2009-07-30 11:55:40 +0200'
  date_gmt: '2009-07-30 09:55:40 +0200'
  content: '[...] during development and not production, I find this tool very useful.
    For more informations visit author&#8217;s blog or tools [...]'
- id: 559
  author: Paul Szulc
  author_email: paul.szulc@gmail.com
  author_url: http://paulszulc.wordpress.com
  date: '2009-07-30 11:57:42 +0200'
  date_gmt: '2009-07-30 09:57:42 +0200'
  content: Very nice, I've put info on my blog to spread the word
- id: 609
  author: Alok Pathak
  author_email: alokpathak79@gmail.com
  author_url: ''
  date: '2009-11-09 12:01:27 +0100'
  date_gmt: '2009-11-09 10:01:27 +0100'
  content: "Hello,\r\n\r\nI have modified the Wicket-Bench plugin to satisfy my needs,
    I tested the plugin on Eclipse 3.5, 3.4, 3.3.\r\n\r\nI think the plugin may help
    others also. How can I share It.\r\n\r\nFunctionality I added:\r\n\r\n   1. A
    place to provide \"Context Path\" In the New Project Wizard.\r\n   2. Added latest
    Wicket 1.4.3 jar to the project.\r\n   3. Added Jetty Support\r\n   4. Added a
    class named \"RunProject\" to run the project using Jetty\r\n   5. Added \"build/build.xml\"
    to create a war that can be deployed On Apache Tomcat.\r\n\r\n\r\nRemoved few
    Bugs.\r\n\r\nI have no Idea about version control, so please forgive me.\r\n\r\nAlok
    Pathak"
---
<p>I just performed the 1.2 release of <a href="http://github.com/dashorst/wicket-stuff-markup-validator">WicketStuff HTML Validator</a>, thanks to Emond Papegaaij, a Topicus Onderwijs co-worker. This new release adds the ability to ignore Wicket specific bugs when using the &amp; entity in JavaScript calls, and adds the ability to ignore &lt;input type="text" autocomplete="false" /&gt; validation errors. Even though you specify that your document is (X)HTML compliant, browsers will (fortunately) still work with these validation errors present.</p>
<p>The markup validator was <a href="http://www.slideshare.net/dashorst/keep-your-wicket-application-in-production/32">featured in my presentation</a> "Get your Wicket Application in production (and keep your weekends free)". The validator will notify you if you have invalid HTML markup in your rendered pages. While usually not destructive, having invalid markup can result in long debugging errors when replacing part of your page with for example Ajax, or traversing the DOM.</p>
<p>You can use it by adding the HtmlValidationResponseFilter to the Wicket<br />
request cycle filters in the following fashion:</p>

{% highlight java %}
public class MyApplication extends WebApplication {
    // ...

    @Override
    protected void init() {
        // only enable the markup filter in DEVELOPMENT mode
        if(DEVELOPMENT.equals(getConfigurationType())) {
            HtmlValidationResponseFilter htmlvalidator = 
                              new HtmlValidationResponseFilter();
            htmlvalidator.setIgnoreAutocomplete(true);
            htmlvalidator.setIgnoreKnownWicketBugs(true);
            getRequestCycleSettings()
                .addResponseFilter(htmlvalidator);
        }
    }
}
{% endhighlight %}

<p>And you&#8217;re all set. Make sure you define a XHTML doctype in your pages, such<br />
as:</p>

{% highlight html %}
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title>Foo</title>
</head>
<body>
</body>
</html>
{% endhighlight %}

<p>To use the HtmlValidator, include the following in your project&#8217;s pom:</p>

{% highlight xml %}
<dependency>
    <groupId>org.wicketstuff</groupId>
    <artifactId>htmlvalidator</artifactId>
    <version>1.2</version>
    <scope>test</scope>
</dependency>
{% endhighlight %}

<p>You might need to add the <a href="http://wicketstuff.org/maven/repository">Wicket Stuff maven repository</a> to your repository list.</p>
