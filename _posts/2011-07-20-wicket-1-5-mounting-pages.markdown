---
layout: post
status: publish
published: true
title: Wicket 1.5 - Mounting pages
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 618
wordpress_url: http://wicketinaction.com/?p=618
date: '2011-07-20 17:58:48 +0200'
date_gmt: '2011-07-20 15:58:48 +0200'
categories:
- Wicket
tags:
- Wicket
- '1.5'
- mapper
comments:
- id: 1078
  author: Alex
  author_email: alex.objelean@gmail.com
  author_url: ''
  date: '2011-07-24 09:43:02 +0200'
  date_gmt: '2011-07-24 07:43:02 +0200'
  content: 'Great article! One suggestion though: the code should be highlighted for
    easier readability. You can use syntaxhighlighter js library for that.'
- id: 1079
  author: martin-g
  author_email: martin.grigorov@gmail.com
  author_url: ''
  date: '2011-07-24 11:44:49 +0200'
  date_gmt: '2011-07-24 09:44:49 +0200'
  content: "Thanks! \r\nI'm just a guest blogger here but I'll ask Martijn to setup
    something.\r\n<strong>Update</strong>: I updated the code snippets."
---
<p>In the previous <a href="http://wicketinaction.com/2011/07/wicket-1-5-request-mapper/">article</a> I described how <em title="org.apache.wicket.request.IRequestMapper">IRequestMapper</em>s work. In this installment I'll show you how to mount pages at nice looking urls.</p>
<p>In Wicket 1.4 there were several implementations of <em title="org.apache.wicket.request.target.coding.IRequestTargetUrlCodingStrategy">IRequestTargetUrlCodingStrategy</em> with which the developer was able to mount a page at a given path and the request parameters were encoded/decoded  depending on the chosen coding strategy. E.g. with <em title="org.apache.wicket.request.target.coding.QueryStringUrlCodingStrategy">QueryStringUrlCodingStrategy</em> the parameters were read/written in the request's query string - /mount/path?param1=value1¶m2=value2, with <em title="org.apache.wicket.request.target.coding.IndexedParamUrlCodingStrategy">IndexedParamUrlCodingStrategy</em> the parameters are represented as part of the request path - /mount/path/param1/param2, with <em title="org.apache.wicket.request.target.coding.MixedParamUrlCodingStrategy">MixedParamUrlCodingStrategy</em> it was possible to use both query string based and indexed based ones.</p>
<p>In Wicket 1.5 all these are combined in MountedMapper. It can encode/decode the parameters in the query string, as indexed and as named indexed.</p>
<p>To mount a page with MountedMapper you should do something like:<br />
MyApp.java</p>
{% highlight java %}public void init() {
  super.init();
  mountPage("/mount/path", MyPage.class);
  // #mountPage() is a shortcut for:
  // mount(new MountedMapper("/mount/path", MyPage.class));
}{% endhighlight %}
<p>Now a request to <em>http://host/mount/path</em> will be handled by MyPage and it wont have any parameters. A request to <em>http://host/mount/path/indexed?queryNamed=queryValue</em> will have one indexed and one named parameters. They can be read with <em title="org.apache.wicket.request.mapper.parameter.PageParameters">PageParameters</em>#get(0) and #get("queryNamed") respectfully.</p>
<p>The new functionality in 1.5 is that now you can have indexed parameters which have names.<br />
If you use <em>mountPage("/mount/path/${named1}/${named2}", MyPage.class);</em> then the request url may look like <em>http://host/mount/path/value1/value2</em> and the parameters' values can be read with PageParameters#get("named1") and #get("named2"). MyPage will handle this request only if both parameters are provided. If 'named2' is not provided then the request mapper wont match and will let another mapper to handle the request.<br />
One more cool thing is that you can have optional indexed named parameters. The difference with mandatory ones is that they use hash sign instead of dollar sign - #{optional}. A mapping like <em>mountPage("/mount/path/${named1}/#{named2}", MyPage.class); </em> will match for both requests <em>http://host/mount/path/value1</em> and <em>http://host/mount/path/value1/value2</em>.</p>
<p>To mount all pages in a package now you should use:<br />
MyApp.java</p>
{% highlight java linenos %}
public void init() {
  super.init();
  mountPackage("package", com.example.pages.SomePage.class);
}
{% endhighlight %}
<p>With this a request to <em>http://host/package/AnotherPage</em> will be handled by AnotherPage only if it is in the same package as <em>SomePage</em>, i.e. <em>com.example.pages</em>.</p>
<p>The last mapper which we will discuss today is <em title="org.apache.wicket.request.mapper.BookmarkableMapper">BookmarkableMapper</em>. This is the mapper which is used when there is no explicit mapping for a page. For example using <em>setResponsePage(com.example.pages.NotMappedPage.class)</em> will produce an url like <em>http://host/wicket/bookmarkable/com.example.pages.NotMappedPage</em>. This functionality is the same as <em>?wicket:bookmarkable=com.example.pages.NotMappedPage</em> in Wicket 1.4. To change the namespace - <em>wicket/bookmarkable</em> - you'll have to setup custom <em title="org.apache.wicket.request.mapper.IMapperContext">IMapperContext</em> by overriding <em title="org.apache.wicket.Application.newMapperContext()">Application.newMapperContext()</em>.</p>
<p>For more information about other mappers provided by Wicket read <a href="https://cwiki.apache.org/confluence/x/sCJkAQ">Request Mappers</a> wiki page.<br />
Some code exploring this topic can be found at: <a href="http://www.wicket-library.com/wicket-examples/mappers/">Mappers examples</a>.</p>
<p>In the next article I'll show you how to mount resources.</p>
