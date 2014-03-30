---
layout: post
status: publish
published: true
title: Wicket 6.0.0-beta2 released
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 739
wordpress_url: http://wicketinaction.com/?p=739
date: '2012-05-31 11:20:53 +0200'
date_gmt: '2012-05-31 09:20:53 +0200'
categories:
- Releases
- Wicket
tags: []
comments: []
---
<p>The Wicket team is proud to announce the second beta release of the Wicket 6.x series. This release brings over many improvements over the 1.5.x series.</p>
<h3 id="new_and_noteworthy">New and Noteworthy</h3>
<h4 id="wicket_atmosphere">Wicket Atmosphere</h4>
<p>The Beta 2 contains a new experimental module Wicket Atmosphere, which brings serverside push to Wicket and provides a great way to render serverside markup and send it to the browsers of your users. Check out the atmosphere example in our Examples project to see it in action.</p>
<p>In your application’s init method you need to register the push event bus:</p>

{% highlight java %}
new EventBus(this);
{% endhighlight %}

<p>Somewhere where you want to push your changes to the client, you need to publish your event to the push<code>EventBus</code>:</p>

{% highlight java %}
EventBus.get().post(input.getModelObject());
{% endhighlight %}

<p>And finally you need to subscribe your page (or component) to the <code>EventBus</code>’s events with <code>@Subscribe</code>, taking in the typed parameter you post to the EventBus (in this case a <code>String</code>):</p>

{% highlight java %}
@Subscribe
public void receiveMessage(AjaxRequestTarget target, String message) {
    label.setDefaultModelObject(message);
    target.add(label);
}
{% endhighlight %}

<p>To be able to use Wicket Atmosphere you need to include the following dependency:</p>

{% highlight xml %}
<dependency>
    <groupId>org.apache.wicket</groupId>
    <artifactId>wicket-atmosphere</artifactId>
    <version>0.1</version>
</dependency>
{% endhighlight %}

<p>Please note that this is still experimental.</p>
<h3 id="this_release">This release</h3>
<p>Check the <a href="https://cwiki.apache.org/WICKET/wicket-60-roadmap.html">roadmap</a> with a list of the major goals. And the <a href="https://cwiki.apache.org/WICKET/migration-to-wicket-60.html">migration guide</a> with all major and some minor changes between 1.5.x and 6.x series.</p>
<p>The Jira changelog of all closed ticket at <a href="https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310561&amp;version=12320343">Jira</a></p>
<p>To use it in Maven:</p>

{% highlight xml %}
<dependency>
    <groupId>org.apache.wicket</groupId>
    <artifactId>wicket-core</artifactId>
    <version>6.0.0-beta2</version>
</dependency>
{% endhighlight %}

<p>If you don’t use a dependencies management build tool then you can download the <a href="http://www.apache.org/dyn/closer.cgi/wicket/6.0.0-beta2">full distribution</a> (including source).</p>
<p>There are no more planned API breaks but if you find something that can be made better now it the time to discuss it! We will try to avoid making any API changes in the Release Candidates that will follow this beta release.</p>
<p>Any feedback about the new features, their implementation and their documentation is very welcome!</p>
<p>The Wicket team!</p>
