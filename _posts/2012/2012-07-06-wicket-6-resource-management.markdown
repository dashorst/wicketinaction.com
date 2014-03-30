---
layout: post
status: publish
published: true
title: Wicket 6 resource management
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 790
wordpress_url: http://wicketinaction.com/?p=790
date: '2012-07-06 15:37:18 +0200'
date_gmt: '2012-07-06 13:37:18 +0200'
categories:
- Wicket
tags: []
comments: []
---
<p>Several improvements in the header contribution functionality have been made in Wicket 6 to make it more flexible.</p>
<h2>Header items</h2>
<p>The major API break is that now IHeaderResponse can render only instances of <em title="org.apache.wicket.markup.head.HeaderItem">HeaderItem</em>. In previous versions IHeaderResponse had many #renderXyz() methods for each and every combination of CSS or JavaScript attributes, like media, IE condition, charset, url, id, etc. The new version of <em title="org.apache.wicket.markup.head.IHeaderResponse">IHeaderResponse</em> has just one <em>#render(HeaderItem)</em> and the developer should use the most specific implementation of HeaderItem.</p>
<p>For example:</p>
<p>AnyComponent/AnyBehavior.java:</p>

{% highlight java %}

@Override
public void renderHead(IHeaderResponse response) {
	super.renderHead(response);

	response.render(new CssContentHeaderItem(someCss, someId, optionalCondition));
	response.render(CssHeaderItem.forReference(new SomeCssResourceReference()));
	response.render(JavaScriptHeaderItem.forScript(someScript, someId));
}

{% endhighlight %}

<p><em title="org.apache.wicket.markup.head.JavaScriptHeaderItem">JavaScriptHeaderItem</em> and <em title="org.apache.wicket.markup.head.CssHeaderItem">CssHeaderItem</em> provide factory methods for all default implementations of JavaScript and CSS related header items.</p>
<h2>Dependency management</h2>
<p>Resource references take advantage of the new HeaderItem's and use them to specify on what other resources they depend. The most trivial example is when a JavaScriptResourceReference used to contribute a jQuery plugin needs to declare that it depends on jQuery. This way the component/behavior needs to contribute only the resources it really depends on and Wicket cares to deliver the dependencies first and then the dependants.</p>

{% highlight java %}
public class MyResourceReference extends JavaScriptResourceReference {
	public ResourceReferenceA() {
		super(ResourceReferenceA.class, "my.js");
	}

	public Iterable<? extends HeaderItem> getDependencies() {
		List<HeaderItem> dependencies = new ArrayList<HeaderItem>();

		Url dojoGoogleCdn = Url.parse("https://ajax.googleapis.com/ajax/libs/dojo/1.7.2/dojo/dojo.js");
		ExternalUrlResourceReference externalUrlResourceReference = new ExternalUrlResourceReference(dojoGoogleCdn);
		dependencies.add(JavaScriptHeaderItem.forReference(externalUrlResourceReference));

		dependencies.add(CssHeaderItem.forReference(new CssResourceReference(ResourceReferenceA.class, "a.css")));

		return dependencies;
	}
}
{% endhighlight %}

<p>With the example above Wicket will render first the &lt;script&gt; to load Dojo and the &lt;link&gt; to load <em>a.css</em> before rendering the &lt;script&gt; for <em>my.js</em>.</p>
<h2>Resource bundles</h2>
<p>Another new feature in Wicket 6 is that several resources can be combined together in one before delivering it to the browser.</p>
<p>One way to do this is to use WebApplication's ResourceBundles:</p>

{% highlight java %}
public void init() {
	super.init();

	getResourceBundles()
		.addJavaScriptBundle(ResourceManagementApplication.class, "bundle.js",
			new ResourceReferenceA(),
			new ResourceReferenceB(),
			new ResourceReferenceC()
		);
}
{% endhighlight %}

<p>Another way is by extending <em title="org.apache.wicket.resource.bundles.ConcatResourceBundleReference">ConcatResourceBundleReference</em> and using it as any other ResourceReference.</p>
<p>The bundle (with all resources) will be contributed if at least one of its resources is contributed by a component or behavior.</p>
<h2>Resource replacement</h2>
<p>Sometimes a third-party library or even Wicket itself adds dependencies you<br />
would rather replace by something else. For example, your application might<br />
depend on a more recent version of a JavaScript library or you want to use a<br />
CDN rather than a packaged resource. For this, Wicket 6 supports replacement<br />
resources. A replacement resource is a resource that will replace any<br />
occurence of some other resource throughout the application. It does not<br />
matter if the replaced resource is added in third party code, in your code or<br />
even as a depedency of another resource.</p>
<p>The following code replaces any occurence of DojoResourceReference by the<br />
version hosted on the Google APIs CDN:</p>
<p>MyApplication.java:</p>

{% highlight java %}
@Override
public void init() {
  super.init();
  
  addResourceReplacement(
    DojoResourceReference.get(),
    new UrlResourceReference(Url.parse("https://ajax.googleapis.com/ajax/libs/dojo/1.7.3/dojo/dojo.js"))
  );
{% endhighlight %}

<h2>Positioning of contributions</h2>
<h3>Filtered items</h3>
<p>A not well know feature in Wicket is that the application can provide custom <em title="org.apache.wicket.markup.html.IHeaderResponseDecorator">IHeaderResponseDecorator</em> and filter the header contributions. A common use case is when the application needs to render all JavaScripts in the footer of the page and leave CSSs in the header.<br />
This is made a bit simpler now by introducing <em title="org.apache.wicket.markup.head.filter.FilteredHeaderItem">FilteredHeaderItem</em>.</p>
<p>To make a use of it the developer have to add a special component in his page:</p>

{% highlight java %}
add(new HeaderResponseContainer("someId", filterName));
{% endhighlight %}

{% highlight html %}
<wicket:container wicket:id="someId"/>
{% endhighlight %}

<p>And later contribute FilteredHeaderItems in any component or behavior:</p>

{% highlight java %}
@Override
public void renderHead(IHeaderResponse response) {
	super.renderHead(response);

	response.render(new FilteredHeaderItem(wrappedItem, filterName));
}
{% endhighlight %}

<h3>Priority items</h3>
<p>Since Wicket 1.5 components do their header contribution before the page they are contained in. The reason is that the application may use a third party component library and the page should be able to override any contribution made by the components. A better design is if the components may be configured what to contribute but sometimes this is not possible. The same is valid for component inheritence - the specialization (the child component) contributes after the base component.<br />
Thanks to the HeaderItems with Wicket 6 it is possible to give some header contributions bigger priority than the others when there is such need. By default Wicket provides <em title="org.apache.wicket.markup.head.PriorityHeaderItem">PriorityHeaderItem</em> which is an item that is contributed before any other non-priority item. If the application needs finer control then it may register a custom comparator with <em title="org.apache.wicket.settings.IResourceSettings">IResourceSettings#setHeaderItemComparator(Comparator)</em></p>

{% highlight java %}
response.render(new PriorityHeaderItem(wrappedItem));
{% endhighlight %}

<h2>Demo application</h2>
<p>A demo application showing all these features in action can be found at Martin Grigorov's GitHub <a href="https://github.com/martin-g/blogs/tree/master/wicket6-resource-management">repository</a>. Run it locally and check the produced markup for the pages. There are comments inside the code describing the demonstrated features.</p>
<p>Additionally the <a href="http://www.wicket-library.com/wicket-examples-6.0.x/resourceaggregation/">Resource aggregation</a> example has been updated to use the new features. </p>
