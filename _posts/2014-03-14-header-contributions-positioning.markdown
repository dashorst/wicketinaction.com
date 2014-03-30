---
layout: post
status: publish
published: true
title: Header contributions positioning
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 1020
wordpress_url: http://wicketinaction.com/?p=1020
date: '2014-03-14 14:47:14 +0100'
date_gmt: '2014-03-14 12:47:14 +0100'
categories:
- Wicket
tags: []
comments: []
---

Wicket 6.0.0 came with some nice <a
href="http://wicketinaction.com/2012/07/wicket-6-resource-management/"
title="Wicket 6 resource management" target="_blank">improvements</a>
in the header contribution management.

With these improvements it is possible to put a specific header
contribution at the top of all others by using <em
title="org.apache.wicket.markup.head.PriorityHeaderItem">PriorityHeaderItem</em>,
or to group several header contributions in a specific spot
in the page body by using <em
title="org.apache.wicket.markup.head.filter.FilteredHeaderItem">FilteredHeaderItem</em>,
but still it was quite hard to something simple like
making sure that &lt;meta charset="utf-8"/&gt; is always the very first
item in the page &lt;head&gt;.

<strong>&lt;wicket:header-items/&gt;</strong>

With <a href="https://issues.apache.org/jira/browse/WICKET-5531"
target="_blank">WICKET-5531</a> (since Wicket 6.15.0) we added one more
facility that should make header contributions management even more
flexible.

By using markup like the one below in YourPage.html

{%highlight html%}
<head>
  <meta chartset="UTF-8"/>
  <wicket:header-items/>
  <script src="my-monkey-patch-of-wicket-ajax.js"></script>
</head>
{%endhighlight%}

all header contributions done by using <em
title="org.apache.wicket.markup.head.IHeaderResponse">IHeaderResponse</em>
in your Java code or the special <a
href="https://cwiki.apache.org/confluence/display/WICKET/Wicket's+XHTML+
tags#Wicket'sXHTMLtags-Elementwicket:head"
target="_blank">wicket:head</a> tag will be put between the
&lt;meta&gt; and &lt;script&gt; elements, i.e. in the place of
&lt;wicket:header-items/&gt;.

This way you can make sure that some header item is always before or
after the header items managed by Wicket.

&lt;wicket:header-items/&gt; can be used only in the page's
&lt;head&gt; element and there could be at most one instance of it.
