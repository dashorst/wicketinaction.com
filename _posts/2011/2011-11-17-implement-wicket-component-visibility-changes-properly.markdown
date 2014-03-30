---
layout: post
status: publish
published: true
title: Implement Wicket component visibility changes properly
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
excerpt: Igor thoughts on how to <em>properly</em> implement Wicket component visibility
  changes taken from the Wicket user list.
wordpress_id: 685
wordpress_url: http://wicketinaction.com/?p=685
date: '2011-11-17 18:31:58 +0100'
date_gmt: '2011-11-17 16:31:58 +0100'
categories:
- Wicket
tags:
- Visibility
- Components
- Coding style
comments: []
---
<p>Igor mentioned this in a thread on our user mailing list on how to <em>properly</em> implement component visibility changes. I figured that this demanded a greater audience so I took the liberty of republishing his thoughts here.</p>
<p>If the component itself controls its own visibility the best way is to override <code>onConfigure()</code> and call <code>setVisible()</code> inside.</p>
<p>If an outside component controls another's visibility the best way is to override the controlling component's <code>onConfigure()</code> and call <code>controlled.setVisible()</code></p>
<p>If you have a simple state toggle, like "when i click this link i want this component's visibility to change" then calling <code>component.setVisible()</code> is fine.</p>
<p>If you have a cross-cutting concern (authorization strategy, configure listener, before render listener, etc) overriding component's visibility then call <code>component.setVisibilityAllowed()</code>. Wicket <code>and</code>s this bit with the visible bit to determine final visibility; this way the cross-cutting concern won't dirty component's visibility attribute.</p>
<p>If all of these fail, as a last resort override <code>component.isVisible()</code> but be warned that this has a few pitfalls:</p>
<ul>
<li>it is called multiple times per request, potentially tens of times, so keep the implementation computationally light</li>
<li>this value should remain stable across the render/respond boundary. Meaning if <code>isVisible()</code> returns true when rendering a button, but when the button is clicked returns false you will get an error</li>
</ul>
