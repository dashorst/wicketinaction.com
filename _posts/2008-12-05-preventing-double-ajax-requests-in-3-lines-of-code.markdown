---
layout: post
status: publish
published: true
title: Preventing double Ajax requests in 3 lines of code
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
excerpt: "One question is recurring on our user list: how can I prevent a user from
  clicking an Ajax link multiple times? There are several solutions to this problem,
  but today I thought up possibly the simplest solution yet for Wicket applications
  using the infrastructure that is already in place for displaying custom Ajax indicators.\r\n"
wordpress_id: 302
wordpress_url: http://wicketinaction.com/?p=302
date: '2008-12-05 17:13:25 +0100'
date_gmt: '2008-12-05 15:13:25 +0100'
categories:
- Wicket
tags:
- Ajax
- Veil
- Indicator
comments:
- id: 218
  author: Bruno Borges
  author_email: bruno.borges@gmail.com
  author_url: http://blog.brunoborges.com.br
  date: '2008-12-05 18:22:17 +0100'
  date_gmt: '2008-12-05 16:22:17 +0100'
  content: "Forget about IE6. It must die anyway!! :-D\r\n\r\nGood example, btw."
- id: 220
  author: Cemal
  author_email: you_have_mine@fakeEmail.com
  author_url: http://www.jWeekend.co.uk
  date: '2008-12-05 20:19:09 +0100'
  date_gmt: '2008-12-05 18:19:09 +0100'
  content: "Martijn,\r\n\r\nThis seems to works well in the newer browsers.\r\nCan't
    beat a smart but simple solution!\r\nIf you don't want to scare your user(s) too
    much, adding \r\nopacity:0.5;filter:alpha(opacity=50); \r\ndoesn't seem to break
    it.\r\n\r\nRegards - Cemal"
- id: 434
  author: tee
  author_email: daniel.teske@web.de
  author_url: ''
  date: '2009-02-26 15:38:45 +0100'
  date_gmt: '2009-02-26 13:38:45 +0100'
  content: "yess! that's perfect! realy cool\r\nanother hint: use css-classes! but
    in this case, it looks like the \"display:none;\"-setting MUST be a part of the
    style-Attribute.\r\n\r\nfor a transparent-grey veil-layer you can use the transparent2.png
    from the ModalWindow-Component\r\nbackground: url('../img/transparent2.png') top
    left;\r\n\r\nto get a loading-animation inside the veil-layer you can put another
    div inside the veil-div whith the style\r\nbackground: url('../img/ajax-loader.gif')
    center no-repeat;\r\n\r\nto get the ajax-loader.gif just try: http://www.ajaxload.info/\r\n\r\nIAjaxIndicatorAware
    ROCKS!"
- id: 665
  author: Jeff Singer
  author_email: jeff@broccoliproject.org
  author_url: ''
  date: '2010-03-17 01:29:00 +0100'
  date_gmt: '2010-03-16 23:29:00 +0100'
  content: "Hi tee\r\n\r\nDid you get the ajax-loader.gif to work?   I have tried
    everything I could think of, but with no success? I can show the image, but it
    does not animate.\r\n\r\nIt seems like there is a incompatibility between the
    display:none and the animated gif. \r\n\r\nIf I make the display: \"\"     then
    the panel shows, the gif animates correctly.\r\n\r\nAny help would be greatly
    appreciated....\r\n\r\nJeff"
---
<p>One question is recurring on our user list: how can I prevent a user from clicking an Ajax link multiple times? There are several solutions to this problem, but today I thought up possibly the simplest solution yet for Wicket applications using the infrastructure that is already in place for displaying custom Ajax indicators.<br />
<a id="more"></a><a id="more-302"></a><br />
Wicket provides the <code>IAjaxIndicatorAware</code> interface where you can identify a piece of markup that is displayed when Wicket is processing an Ajax request, and hidden when the request was completed. Letting your page implement this interface and including a coverall <code>div</code> in your markup is enough to stop your users from sending multiple requests quickly after one another. Take a look at the following Wicket page:</p>

{% highlight java linenos %}
package com.mycompany;

import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.ajax.*;
import org.apache.wicket.ajax.markup.html.*;

public class HomePage extends WebPage 
                      implements IAjaxIndicatorAware {
    public HomePage() {
        add(new AjaxLink("link") {
            public void onClick(AjaxRequestTarget t) {
                try { Thread.sleep(5000); } catch(Exception e) {}
            }
        });
    }
    public String getAjaxIndicatorMarkupId() {
        return "veil";
    }
}
{% endhighlight %}

<p>Line 8 lets our page implement the necessary interface, and lines 16-18 implement the required method provided by this interface. In this method we return the markup identifier (DOM id) for our coverall, in this case called <code>veil</code>. The <code>AjaxLink</code> just sleeps for 5 seconds in the event handler, but you can do your dirty work there. This is just an example of course.</p>
<p>
In our markup we now need to create the coverall so that Wicket is able to show and hide this veil. The markup in the next example shows the complete page markup:</p>

{% highlight html %}
<html>
    <head>
        <title>Wicket Quickstart Archetype Homepage</title>
    </head>
    <body>
        <a href="#" wicket:id="link">Show veil</a>
        <div id="veil" style="display:none;position:absolute;top:0;left:0;z-index=99999;background-color:black;width:100%;height:100%;color:white">
            <h1>Can't touch this</h1>
        </div>
    </body>
</html>
{% endhighlight %}

<p>And that is all there's to it. Done. Finito. Ready. Complete. Can this be improved? Yes:</p>
<ul>
<li>Normal requests aren't covered (see a <a href="http://cwiki.apache.org/WICKET/generic-busy-indicator-for-both-ajax-and-non-ajax-submits.html">possible solution</a>)</li>
<li>IE6 is a bitch</li>
<li>Styling could be better</li>
</ul>
<p>And probably more, but for normal browsers and just Ajax requests, this is the easiest way to hack this in.</p>
