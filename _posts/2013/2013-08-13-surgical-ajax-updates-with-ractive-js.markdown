---
layout: post
status: publish
published: true
title: Surgical Ajax updates with Ractive.js
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 871
wordpress_url: http://wicketinaction.com/?p=871
date: '2013-08-13 14:43:59 +0200'
date_gmt: '2013-08-13 12:43:59 +0200'
categories:
- Wicket
tags: []
comments: []
---

### Replace all with outerHTML

At the moment (Apache Wicket version 6.10.0) when a Component has to be
updated in Ajax response Wicket will replace the whole HTML element
with all its attributes and sub-elements included, i.e. the
<em>outerHTML</em> of the element is replaced.

Wicket has always tried to do this in the most efficient way for the
different browsers. Since version 6.0.0 Wicket uses jQuery to do this,
so jQuery cares about the browser inconsistencies.

With or without using jQuery for DOM manipulations if the change in the
DOM is as minimal as a new value for some attribute of the HTML element
then Wicket will replace the whole element (including its sub-elements)
to be able to update the attribute. Depending on the size of the markup
this may be a noticeable or not.

### Replace only what has changed with Ractive.js

In this article I want to introduce you <a
href="http://www.ractivejs.org/">Ractive.js</a> - a JavaScript library
that among other things is able to make surgical updates in the DOM
tree.

Ractive.js uses templating based on <a
href="http://mustache.github.io/">Mustache</a>. For example your markup
template can look like:

{% highlight html %}
<p id="myTmpl" style="color: {{user.color}}">Hello {{user.name}}! You have
    <strong>{{user.messages.unread}} new messages</strong>.
</p>
{% endhighlight %}
<p>To populate the data in the mustaches you need to initialize a Ractive instance with something like this:</p>
{% highlight javascript %}
var ractive = new Ractive({
  el: "#target",
  template: "myTmpl",
  data: {
    user: {
      color: "green",
      name: 'Dave',
      messages: { total: 11, unread: 4 }
    }
  }
});
{% endhighlight %}

This will read the template from the element with id "myTmpl" and put
the final result in an element with id "target".

Later if you need to update some variable (mustache) you should do:

{% highlight javascript %}
ractive.set('user.messages.unread', 2);
{% endhighlight %}

This will update just the text node inside the <em>strong</em> HTML
element responsible to visualize the number of the unread messages.

Even if we do something like:

{% highlight javascript %}
ractive.set('user', {
  color: "green",
  name: "Dave",
  messages: {
    total: 11,
    unread: 2
  }
});
{% endhighlight %}

Note that only the *unread* has a new value, Ractive.js will figure
that out and will do the right thing - will update the same text node
only, as above, not the complete template.

### The integration

At <a
href="https://github.com/martin-g/wicket-ractive/">Wicket-Ractive</a>
you may find an integration between Wicket and Ractive that takes
advantage of Ractive's DOM update possibilities.

By adding `RactiveBehavior` to a component/panel Wicket will use
its model object to create an instance of Ractive and populate its
mustaches with initial values from the object's properties. Later when
updating this component you may use
`Ractive#ractive(AjaxRequestTarget, Component)` helper to tell
Ractive to do its magics and update only the bean properties which have
new values.

`Ractive#ractive(AjaxRequestTarget, Component)` helper can be used when
you know that the changes in the model object are small enough. In case
there are bigger changes and replacing the *outerHTML* may be
more efficient you can still use
`AjaxRequestTarget#add(Component)` as you do now.

There is a demo application in the `src/test/java` folder of this
project. Try it.

### Conclusion

Ractive.js is used intensively at <a href="http://theguardian.com/">The
Guardian</a> but it is still a new project that has to prove itself
among other good JavaScript libraries.

The usage of the mustaches in the markup template is against Wicket's
philosophy of developing with <em>plain Java and plain HTML</em>, so
this approach may be a step in the wrong direction for some of the
Wicket's users.
