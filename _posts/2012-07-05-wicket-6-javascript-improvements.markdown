---
layout: post
status: publish
published: true
title: Wicket 6 JavaScript improvements
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 762
wordpress_url: http://wicketinaction.com/?p=762
date: '2012-07-05 10:56:25 +0200'
date_gmt: '2012-07-05 08:56:25 +0200'
categories:
- Wicket
tags: []
comments: []
---
<p>A major rework of the internals of the core Wicket JavaScript libraries has been done for Wicket 6. The good news is the client APIs are mostly the same and the application developers will not need to change their applications.</p>
<h2>Event registration</h2>
<p>Until version 1.5.x attaching an Ajax event behavior to a component would lead to addition of an inline attribute to the markup of this component. For example the produced markup for an AjaxLink is similar to:</p>

{% highlight html %}
<a id="linkId" onclick="wicketAjaxGet('some/url')">Link</a>
{% endhighlight %}

<p>In Wicket 6 JavaScript event registration is being used instead. For example the above becomes:</p>

{% highlight html %}
<head>
  ...
  <script>
    Wicket.Event.add(window, 'domready', function(event) {
      Wicket.Ajax.get({'u': 'some/url', 'c': 'linkId', 'e': 'click'}));
      // ... more event registrations and onDomReady scripts
    });
  </script>
</head>
...
<a id="linkId">Link</a>
{% endhighlight %}

<p>The benefit is that the event registrations for all elements are done in one place - the page header and your markup elements are not cluttered with extra attributes. Also the generated code for the event registration is much shorter than the one produced in the earlier versions.</p>
<h2>jQuery as backing library</h1>
<p>The previous implementations of wicket-ajax.js and wicket-event.js were home baked solutions which worked well but also suffered from the differences in the browsers. Often users complained that some functionality doesn't work on particular version of particular browser. That's why the Wicket team chose to use JQuery to deal with browser inconsistencies and leave us to do our business logic.</p>
<p>All Java components and behaviors still use the Wicket.** APIs so if an application already uses another big JavaScript library and adding jQuery is not an option then the developer of this application will be able to provide implementation of wicket-event.js and wicket-ajax.js based on her favorite JavaScript library.<br />
To do it the developer should use the new <em title="org.apache.wicket.settings.IJavaScriptLibrarySettings">IJavaScriptLibrarySettings</em>:</p>
<p>MyApplication#init():</p>

{% highlight java %}

public void init() {
  super.init();

  IJavaScriptLibrarySettings jsSettings = getJavaScriptLibrarySettings();

  jsSettings.setBackingLibraryReference(new DojoResourceReference());

  jsSettings.setWicketEventReference(new DojoWicketEventResourceReference());

  jsSettings.setWicketAjaxReference(new DojoWicketAjaxResourceReference());

}
{% endhighlight %}

<h2>Ajax request attributes</h2>
<p>The main change in the Java code is the introduction of <em title="org.apache.wicket.ajax.attributes.AjaxRequestAttributes">AjaxRequestAttributes</em> in Ajax behaviors API.<br />
AjaxRequestAttributes can be used to configure how exactly the Ajax call should be executed and how its response should be processed. To do this use:</p>
<p>AnyAjaxComponent/AnyAjaxBehavior.java:</p>

{% highlight java %}
  @Override
  protected void updateAjaxAttributes(AjaxRequestAttributes attributes)
  {
      super.updateAjaxAttributes(attributes);

      attributes.setSomeAttribute();
  }
{% endhighlight %}

<p>This way the developer can specify whether the Ajax call should use GET or POST method, whether to submit a form, on what JavaScript events to listen to, etc. For a full list of the available attributes see the new <a href="https://cwiki.apache.org/WICKET/wicket-ajax.html#WicketAjax-AjaxRequestAttributes">Wiki</a> page. Most of the attributes have default values and they may be omitted in the generated JavaScript code.</p>
<h2>Ajax call listeners</h2>
<p>Because of the usage of event registration there is no need of <em title="org.apache.wicket.ajax.IAjaxCallDecorator">IAjaxCallDecorator</em> because there is no script to decorate anymore. The same functionality can be achieved with the new <em title="org.apache.wicket.ajax.attributes.IAjaxCallListener">IAjaxCallListener</em>. It provides the means to register JavaScript listeners which are being called during the Ajax call execution. A listener may provide a <em>precondition</em> which may cancel completely the Ajax call when evaluated to false, a <em>before handler</em> - executed right after the successful pass of the preconditions, an <em>after handler</em> - executed right after the make of the Ajax call, a <em>success handler</em> - executed on successful return of the Ajax call, a <em>failure handler</em> - executed when the Ajax call fails for some reason, and a <em>complete handler</em> which is executed after the success/failure handlers no matter what.</p>
<p>To register an AjaxCallListener do something like:</p>

{% highlight java %}
  @Override
  protected void updateAjaxAttributes(AjaxRequestAttributes attributes)
  {
      super.updateAjaxAttributes(AjaxRequestAttributes attributes);

      IAjaxCallListener listener = new IAjaxCallListener()
      {
        @Override
        public CharSequence getBeforeHandler(Component c) { return handler; }
        .....
      };

      attributes.getAjaxCallListeners().add(listener);
  }
{% endhighlight %}

<p>In the example above the returned <em>handler</em> could be any instance of CharSequence. If it is an instance of <em title="org.apache.wicket.ajax.json.JsonFunction">JsonFunction</em> then it will be written as is, otherwise Wicket will assume that the text is just the body of a JavaScript function and will wrap it in <em title="org.apache.wicket.ajax.json.JsonFunction">JsonFunction</em> for you. The benefit of using <em title="org.apache.wicket.ajax.json.JsonFunction">JsonFunction</em> is that it generates JSON with <em>function</em> as a value. This is non-valid JSON but this way there is no need to use <em>eval()</em> in the browser and the execution of the handlers is faster.</p>
<h2>Global Ajax call listeners</h2>
<p>IAjaxCallListener's can be used to listen for the lifecycle of an Ajax call for a specific component. If the user application needs to listen for all Ajax calls then it may subscribe to the following topics:</p>
<ul>
<li>/ajax/call/before</li>
<li>/ajax/call/after</li>
<li>/ajax/call/success</li>
<li>/ajax/call/failure</li>
<li>/ajax/call/complete</li>
</ul>
<p>Those replaces the old Wicket.Ajax.(registerPreCallHandler|registerPostCallHandler|registerFailureHandler) methods and uses publish/subscribe mechanism.</p>
<p>Example (JavaScript):</p>

{% highlight javascript %}
    Wicket.Event.subscribe('/ajax/call/failure', function(jqEvent, attributes, jqXHR, errorThrown, textStatus) {
      // do something when an Ajax call fails
    });
{% endhighlight %}

<p>All global listeners receive the same arguments as the respective IAjaxCallListener handler plus jQuery.Event that is passed by the PubSub system.</p>
<h2>Demo application</h2>
<p>At Martin Grigorov's GitHub <a href="https://github.com/martin-g/blogs/tree/master/wicket6-ajax-demo">repository</a> you may find a simple demo application that shows some of the new features.<br />
The application provides a custom AjaxButton (<em title="com.wicketinaction.HandlebarsButton">HandlebarsButton</em>) that uses:</p>
<ul>
<li>org.apache.wicket.ajax.attributes.AjaxRequestAttributes#setWicketAjaxResponse(false) - tells Wicket.Ajax to not try to process the returned Ajax response. The response is custom JSON which will be processed by a custom success handler</li>
<li>org.apache.wicket.ajax.attributes.AjaxRequestAttributes#setDataType("json") - a hint to Wicket.Ajax that the response is JSON and it will try to parse it for us, so your success handler can use it as JavaScript object directly</li>
<li>usage of JsonFunction as success handler - this way the application needs to provide the complete code of a JavaScript function that will handle the success case</li>
<li>usage of plain Strings with the function bodies for the precondition, before and complete handlers - Wicket will wrap them in JsonFunction for you</li>
<li>shows how to suppress the execution of AjaxRequestHandler - there is no need of it since we return custom JSON response</li>
</ul>
<p>The application itself just updates a POJO (an article) at the server side and serializes it to JSON which is used at the client side to be rendered with the help of <a href="http://handlebarsjs.com/">Handlebars</a> JavaScript library.</p>
<h2>More</h2>
<p>More complete documentation may be found at the <a href="https://cwiki.apache.org/WICKET/wicket-ajax.html">Wiki</a>. Feel free to contribute to make it better. Just create an account and edit it!</p>
