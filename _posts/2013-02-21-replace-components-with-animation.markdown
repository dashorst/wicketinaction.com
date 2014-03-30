---
layout: post
status: publish
published: true
title: Replace components with animation
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 829
wordpress_url: http://wicketinaction.com/?p=829
date: '2013-02-21 17:00:49 +0100'
date_gmt: '2013-02-21 15:00:49 +0100'
categories:
- Wicket
tags: []
comments: []
---
<p>In this article I'm going to show you how to replace a component in Ajax response using animation effects like sliding, fading, etc.</p>
<h1>The problem</h1>
<p>To update a <em title="org.apache.wicket.Component">Component</em> in Ajax request Wicket just replaces its old HTML DOM element with a new one created by the HTML in the response.<br />
To update it with an animation you need to do:</p>
<ol>
<li>hide the old HTML element</li>
<li>replace the component, but do not show it</li>
<li>show the new HTML element</li>
</ol>
<p>Attempt 1 (not working):</p>

{% highlight java %}
public void onClick(AjaxRequestTarget target) {
  target.prependJavaScript("jQuery('#"+component.getMarkupId()+"').slideUp(1000);");
  target.add(component);
  target.appendJavaScript("jQuery('#"+component.getMarkupId()+"').slideDown(1000);");
}
{% endhighlight %}

<p>The first problem is that animations in JavaScript are implemented with <em>window.setTimeout</em> or with <em>requestAnimationFrame</em> and this makes them asynchronous-ish. That is Wicket will execute the prepend script to hide the element but wont wait for 1000ms before replacing it, so you wont see the <em>slideUp</em> effect.</p>
<p>The second problem is that <em>target.add(component);</em> will show the new HTML element right away and thus <em>slideDown</em> has nothing to do.</p>
<h1>The solution</h1>
<p>wicket-ajax-jquery.js supports special syntax for the prepended/appended JavaScripts: <em>functionName|some-real-js-code</em>. Such JavaScript is transformed to: <em>function(functionName) {some-real-js-code}</em> where <em>functionName</em> is a function which should be executed in your JavaScript code to allow the next operation to be executed.</p>
<p>Attempt 2 (working):</p>

{% highlight java %}
public void onClick(AjaxRequestTarget target) {
  component.add(new DisplayNoneBehavior());

  target.prependJavaScript("notify|jQuery('#" + component.getMarkupId() + "').slideUp(1000, notify);");
  target.add(component);
  target.appendJavaScript("jQuery('#"+component.getMarkupId()+"').slideDown(1000);");
}
{% endhighlight %}

{% highlight java %}
private static class DisplayNoneBehavior extends AttributeAppender {
  private DisplayNoneBehavior() {
    super("style", Model.of("display: none"));
  }

  @Override
  public boolean isTemporary(Component component) {
    return true;
  }
}
{% endhighlight %}

<p>Problem 1) is solved by using <em>notify</em> as a callback function which should be executed when the sliding up is finished.<br />
Problem 2) is solved by using <em>DisplayNoneBehavior</em> that add 'display: none' to the markup of this component just for this rendering (see Behavior#isTemporary(Component)).</p>
<p>This code can be extracted in a helper class and reused. Simple implementation of such helper class can be found in the <a href="https://github.com/martin-g/blogs/tree/master/wicket6-replace-with-effect">demo application</a>.</p>
<p>This functionality is broken in Wicket 6.4.0, 6.5.0 and 6.6.0. It is fixed with <a href="https://issues.apache.org/jira/browse/WICKET-5039">WICKET-5039</a>.</p>
