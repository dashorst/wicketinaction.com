---
layout: post
status: publish
published: true
title: Repainting only newly-created repeater items via ajax
author: ivaynberg
author_login: ivaynberg
author_email: igor.vaynberg@gmail.com
wordpress_id: 252
wordpress_url: http://wicketinaction.com/?p=252
date: '2008-10-24 01:26:23 +0200'
date_gmt: '2008-10-23 23:26:23 +0200'
categories:
- Wicket
tags:
- Wicket
- Ajax
comments:
- id: 84
  author: Stefan Fussenegger
  author_email: stf@molindo.at
  author_url: ''
  date: '2008-10-24 09:46:30 +0200'
  date_gmt: '2008-10-24 07:46:30 +0200'
  content: "Hi Igor. Yet another great tutorial, thanks. But how would you delete
    an item? Or add a row not at the end but somewhere in the middle? I'd guess that
    it works with plain JS.\r\n\r\n// remove item\r\ntarget.prependJavascript(\r\n
    \       String.format(\r\n        \"Wicket.$('%s').removeChild(Wicket.$('%s'));\",\r\n
    \       \"tr\",container.getMarkupId(),item.getMarkupId()));\r\n\r\n// add after
    existingItem\r\ntarget.prependJavascript(\r\n        String.format(\r\n        \"var
    item=document.createElement('%s');item.id='%s';\"+\r\n        \"Wicket.$('%s').insertBefore(item,
    Wicket.$('%s'));\",\r\n        \"tr\",item.getMarkupId(), container.getMarkupId(),
    oldItem.getMarkupId()));\r\n\r\ngetting the old items from the repeater shouldn't
    be a problem, right? I hope my still-tired brain doesn't fool me here :)"
- id: 85
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-24 17:46:29 +0200'
  date_gmt: '2008-10-24 15:46:29 +0200'
  content: '@Stefan: That''s exactly right. Getting old items is easy, they are all
    direct children of the repeater. Obviously, it gets a little bit more complicated
    for a GridView because there you are looking for grid items that are not direct
    children, but that is true for any high-level repeater that embeds another repeater
    inside.'
- id: 131
  author: Jeff
  author_email: jschwartz@citytechinc.com
  author_url: ''
  date: '2008-11-07 23:54:36 +0100'
  date_gmt: '2008-11-07 21:54:36 +0100'
  content: Great example!  Could this be done with a RefreshingView and an AjaxLink?
- id: 132
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-11-08 00:30:17 +0100'
  date_gmt: '2008-11-07 22:30:17 +0100'
  content: '@Jeff: It should be possible albeit a little harder. You will have to
    iterate over all existing items, see which ones have been removed and added and
    spit out javascript to synchronize server with client. Component#hasBeenRendered()
    can come in handy when figuring out which items have been left over from the previous
    request.'
- id: 151
  author: glenn
  author_email: bingo.smiles@gmail.com
  author_url: ''
  date: '2008-11-16 14:00:39 +0100'
  date_gmt: '2008-11-16 12:00:39 +0100'
  content: "I found this link as helpfulon this. I think I see what i need..\r\nhttp://donteattoomuch.blogspot.com/2008/04/partial-ajax-update-capable-list-view.html"
- id: 491
  author: Matt Shannon
  author_email: mshannon@gmail.com
  author_url: ''
  date: '2009-05-08 08:33:07 +0200'
  date_gmt: '2009-05-08 06:33:07 +0200'
  content: "Nice example, note there is a bug I believe. onSubmit of form, you should
    create a new contact object and associate it with your form model.  Otherwise,
    you will continue to re-use the same single contact instance that was supplied
    on form creation.\r\n\r\n\r\nI created an ajaxlink to remove that seems to work
    ...\r\n\r\n      add(new AjaxLink(\"remove\")\r\n      {\r\n        public void
    onClick(AjaxRequestTarget target)\r\n        {\r\n          String markupId =
    ...\r\n\r\n          target.prependJavascript(\"var item=document.getElementById('\"\r\n
    \           + markupId + \"');Wicket.$('\" + container.getMarkupId()\r\n            +
    \"').removeChild(item);\");\r\n\r\n          contacts.remove(...);\r\n          repeater.remove(...);\r\n
    \       }\r\n      });"
- id: 539
  author: sven
  author_email: sven546@mailinator.com
  author_url: ''
  date: '2009-06-30 15:13:44 +0200'
  date_gmt: '2009-06-30 13:13:44 +0200'
  content: Very nice. But how to get markupId for the remove operation?
- id: 540
  author: sven
  author_email: sven546@mailinator.com
  author_url: ''
  date: '2009-06-30 16:15:41 +0200'
  date_gmt: '2009-06-30 14:15:41 +0200'
  content: "Hi again, I'm having big problems with refresh.\r\nAfter adding the rows
    to dataView (refreshingView in your case), they are displayed correctly. But after
    F5 (refresh), they disappear. \r\nI added 'expanded' property to model item. And
    when populating the item (populateItem), I check it. If it is set, I create the
    new rows with your buildItem method. But as the result, the rows are displayed
    above the current row, which is logical, but wrong. \r\nHow can the rows be displayed
    under the current row (with which I work in populateItem method), please?\r\nThanks
    a lot"
- id: 541
  author: sven
  author_email: sven546@mailinator.com
  author_url: ''
  date: '2009-06-30 16:17:56 +0200'
  date_gmt: '2009-06-30 14:17:56 +0200'
  content: Note - they cannot be added into the collection of contacts (which works
    nicely), because they act as subrows - they are little different then the main
    rows.
- id: 542
  author: sven
  author_email: sven546@mailinator.com
  author_url: ''
  date: '2009-06-30 17:11:36 +0200'
  date_gmt: '2009-06-30 15:11:36 +0200'
  content: For record - maybe I found solution - add them into collection and use
    fragments for the two different styles of rows.
- id: 557
  author: monika
  author_email: monika@biobarlang.hu
  author_url: ''
  date: '2009-07-28 11:45:01 +0200'
  date_gmt: '2009-07-28 09:45:01 +0200'
  content: "A nice tutorial, thanks. However if you want less Javascript magic and
    more Wicket, then I don't think you should be doing such a trick to save a few
    hundred bytes of bandwidth. This blog entry provides a solution for this case:\r\nhttp://oktech.hu/blog/2009/07/adding-and-removing-rows-in-wicket.html"
- id: 562
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-07-30 19:02:03 +0200'
  date_gmt: '2009-07-30 17:02:03 +0200'
  content: '@monika: Based on your table it can be quiet a lot of data. Also based
    on complexity of the data - you might not want to run the server side code/queries
    necessary to rebuild that data. Anyways, the point of this article was to demonstrate
    a partial update to the table, not a partial update to the page which is trivial
    in Wicket.'
- id: 779
  author: Daniel
  author_email: daniel@reuterwall.com
  author_url: ''
  date: '2011-02-09 00:58:39 +0100'
  date_gmt: '2011-02-08 22:58:39 +0100'
  content: "Hi Igor!\r\n\r\nGreat tutorial on a very handy feature.\r\nI tried your
    sample project and it works as expected.\r\nWhen I tried it in my own project
    however, I couldn't get it to work. I keep getting 'unable to find markup for
    the component' errors. I cannot figure it out, do you have any idea without going
    through the code? The JavaScript that adds the new tag works fine but it doesn't
    seem to be found by on the server side.\r\n\r\nI know this post is a bit old,
    have you added and support for this in 1.5?\r\n\r\nThanks"
- id: 809
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2011-03-09 18:03:09 +0100'
  date_gmt: '2011-03-09 16:03:09 +0100'
  content: '@Daniel, you are more then welcome to post a non-working quickstart to
    wicket''s mailing lists. Not much, ajax-wise, has changed between 1.4 and 1.5
    so it should still work.'
- id: 1103
  author: paul
  author_email: n0thing@f0x.com
  author_url: ''
  date: '2011-08-22 20:39:05 +0200'
  date_gmt: '2011-08-22 18:39:05 +0200'
  content: "Thank you for this write-up, it was just what I was looking for.  I believe
    this is a clear indication of the fundamental weakness of Wicket, or more generally,
    most server-centric frameworks.  \r\n\r\nAfter gaining proficiency in Wicket,
    and then engaging in black magic such as this, would the programmer have really
    saved time compared with using even the most basic frameworks (e.g. javascript
    + spring-mvc)?  What about a more powerful framework, such as GWT?  Going further,
    would the project be more maintainable?  \r\n\r\nI personally believe that the
    answer to these questions is \"no\".  In the case of Wicket we've added another
    complicated layer of abstraction (loss of maintainability), and in return we've
    lost performance (many unnecessary hooks from client to server; unnecessary storage
    of client-side state on server, etc)."
- id: 1104
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2011-08-23 00:50:11 +0200'
  date_gmt: '2011-08-22 22:50:11 +0200'
  content: this is not a "weakness" of Wicket in particular. all frameworks would
    suffer this particular problem. Eg how would spring-mvc know where to put the
    new row in markup and what tag should represent the new row, youd have to hand-code
    it just like you would in wicket? maybe the repeater uses different tags based
    on some business logic to represent each row... only the user of the repeater
    knows how to handle this situation. i suppose its possible to integrate this usecase
    better into wicket, maybe something for a later version, feel free to open an
    RFE in our jira.
---
<p>Ajax in Wicket is great, for the most part its usage is transparent and things work as you would expect. Where Ajax does not work perfectly, yet, is when it comes to partially updating repeaters. The problem is that repeater components do not have a markup tag of their own and so Wicket cannot transparently figure out where the updates should go.</p>
<p>Consider a simple case of a repeater rendering a list of contacts. Wicket markup looks like this:</p>

{% highlight html linenos %}
<body>
  <table>
    <tr wicket:id="contacts">
      <td><span wicket:id="name"></span></td>
    </tr>
  </table>
</body>
{% endhighlight %}

<p>And the rendered markup looks like this:</p>

{% highlight java linenos %}
<body>
  <table>
    <tr><td><span>Lisa Simpson</span></td></tr>
    <tr><td><span>Bart Simpson</span></td></tr>
  </table>
</body>
{% endhighlight %}

<p>As you can see, if we add a new item to the <code>ListView</code> and try to repaint it via Ajax there is no root markup tag for the Wicket Ajax to replace. This is why it is necessary to add the repeater to a container and then repaint the container instead. In this case we would add a <code>WebMarkupContainer</code> to the <code>table</code> tag and repaint it via Ajax instead. This, however, will result in all table rows being rendered and sent over from server to client, which for a lot of cases is undesirable.</p>
<p>It is not too hard to support the usecase of "I added something new to the repeater and want to have just that row added via Ajax". The trick is to give Wicket a tag to repaint via Ajax which can be accomplished by doing the following:</p>
<ol>
<li>create the markup tag to represent the new item</li>
<li>add it to the right place in the markup</li>
<li>have Wicket repaint it via Ajax</li>
</ol>
<p>Accomplishing this is pretty easy. Suppose we create the new item by submitting a form via an <code>AjaxButton</code>:</p>

{% highlight java %}
form.add(new AjaxButton("submit")
  {
    @Override
    protected void onSubmit(AjaxRequestTarget target, Form< ? > f)
    {
      // retrieve the newly added contact bean
      Contact contact = form.getModelObject();
      // add it to the collection on serverside
      contacts.add(contact);
      // create the new repeater item and add it to the repeater
      Component item = buildItem(contact);
      // first execute javascript which creates a placeholder tag in markup for this item
      target.prependJavascript(
        String.format(
        "var item=document.createElement(&#039;%s&#039;);item.id=&#039;%s&#039;;"+
        "Wicket.$(&#039;%s&#039;).appendChild(item);",
        "tr", item.getMarkupId(), container.getMarkupId()));

        // notice how we set the newly created item tag&#039;s id to that of the newly created
        // Wicket component, this is what will link this markup tag to Wicket component
        // during Ajax repaint
 
        // all thats left is to repaint the new item via Ajax
        target.addComponent(item);
      }
    });
{% endhighlight %}

<p>When the form is submitted using the above <code>AjaxButton</code> only the newly added item is repainted via Ajax instead of the entire table. A functional project is attached below.</p>
<p>This concept can be taken to the next level where a repeater can be "synced" with the markup on the client side. This would involve:</p>
<ol>
<li>Building a list of item ids currently in the repeater</li>
<li>Invoking #onBeforeRender on the repeater to make it generate new items</li>
<li>Building a list of new item ids in the repeater</li>
<li>Syncing the client markup from old items to new items by deleting removed items and adding new items via the same technique as above</li>
</ol>
<p>Maybe next time :)</p>
<p>Sample project: <a href=&#039;http://wicketinaction.com/wp-content/uploads/2008/10/partialajax.zip&#039;>partialajax.zip</a>
