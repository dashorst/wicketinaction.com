---
layout: post
status: publish
published: true
title: Wicket 1.5 - Request Mapper
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 592
wordpress_url: http://wicketinaction.com/?p=592
date: '2011-07-03 16:45:08 +0200'
date_gmt: '2011-07-03 14:45:08 +0200'
categories:
- Wicket
tags:
- Wicket
- '1.5'
- mapper
comments: []
---
<p>Hi, in series of short articles I'll try to explain the new stuff in Wicket 1.5. It may be a completely new feature or a change since 1.4, hopefully for good.</p>
<p>In this first article I'll talk about the new request mappers. They are semi- new feature, semi-change because they completely replace the functionality that <em title="org.apache.wicket.request.target.coding.IRequestTargetUrlCodingStrategy">IRequestTargetUrlCodingStrategy</em> provided in 1.4.</p>
<p>Simply said, the <em title="org.apache.wicket.Application">Application</em> class has a set of <em title="org.apache.wicket.request.IRequestMapper">IRequestMapper</em>'s and for each request the <em title="org.apache.wicket.request.cycle.RequestCycle">RequestCycle</em> asks these mappers whether any of them knows how to handle the current request's url.<br />
Wicket pre-configures several mappers [<a href="#1">1</a>] which are used for the basic application functionality like a mapper for the home page, a mapper for Wicket resources, for bookmarkable pages, etc.  The user application can add additional mappers with the various <em title="org.apache.wicket.protocol.http.WebApplication">WebApplication#mountXYZ()</em> methods.</p>
<p>The mapper has two main tasks:</p>
<ol>
<li>To create <em title="org.apache.wicket.request.IRequestHandler">IRequestHandler</em> [<a href="#2">2</a>] that will produce the response.<br />
When a request comes Wicket uses <em title="org.apache.wicket.request.IRequestMapper">IRequestMapper#getCompatibilityScore(Request)</em> to decide which mapper should be asked first to process the request. If two mappers have the same score then the one added later is asked first. This way user's mappers have precedence than the system ones. If a mapper knows how to handle the request's url then it should return non-null <em title="org.apache.wicket.request.IRequestHandler">IRequestHandler</em>.</li>
<li>The second task is to produce <em title="org.apache.wicket.request.Url">Url</em> for an <em title="org.apache.wicket.request.IRequestHandler">IRequestHandler</em>.<br />
This is needed at markup rendering time to create the urls for links, forms' action attribute, etc.</li>
</ol>
<p>In the next article I will show how to use most of the default mapper implementations.</p>
<p><span id="1">1</span>. See the source of <em title="org.apache.wicket.SystemMapper">SystemMapper</em> for more details.<br />
<span id="2">2</span>. <em title="org.apache.wicket.request.IRequestHandler">IRequestHandler</em> is what <em title="org.apache.wicket.request.IRequestTarget">IRequestTarget</em> is in Wicket 1.4.</p>
