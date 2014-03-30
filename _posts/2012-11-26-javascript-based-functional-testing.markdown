---
layout: post
status: publish
published: true
title: JavaScript based functional testing
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 815
wordpress_url: http://wicketinaction.com/?p=815
date: '2012-11-26 16:38:08 +0100'
date_gmt: '2012-11-26 14:38:08 +0100'
categories:
- Wicket
tags: []
comments: []
---
<h2>The problem</h2>
<p>Every software developer very well knows that producing new code leads to producing new bugs (problems in the software).<br />
The good software developers know that writing tests for their code doesn"t solve completely the problem but at least prevents regressions. Unfortunately developers often do not have time to write the tests, or are too lazy to do it, or just don"t like the way they do it and try to postpone it.</p>
<p>In this article I"ll describe a prototype of a mini framework that can be used to create functional tests for web applications that run blazing fast, are easy to debug and more imporatantly are very close to the real way a web application is being used.</p>
<p><strong>Note</strong>: this article is not Wicket specific. The described solution can be used for any kind of web application.</p>
<h2>The solution</h2>
<p>A well known library for writing functional tests for web applications is <a href="https://code.google.com/p/selenium/">WebDriver/Selenium2</a>.<br />
My problem with it is that it is:</p>
<ul>
<li>slow - starting the browser instance takes time, issuing commands to it takes time, HtmlUnitDriver with JavaScript enabled is <strong>very</strong> slow...</li>
<li>unreliable - every Driver implementation behaves differently. For me only FirefoxDriver works well</li>
<li>well, let"s not say more bad words for WebDriver. Some of its developers may have the same opinion for Apache Wicket :-)</li>
</ul>
<p>In this article I"m going to describe how to use <a href="http://qunitjs.com/">QUnit</a> to run functional tests for <a href="http://www.wicket-library.com/wicket-examples-6.0.x/">Wicket Examples</a>.</p>
<p>Apache Wicket project already uses QUnit for its <a href="https://github.com/apache/wicket/tree/master/wicket-core/src/test/js">unit tests</a> since version 6.0.0 and so far they seems to serve us very well. </p>
<p>The main idea is that the tests run the application in an iframe, i.e. each test tells the iframe what is its initial source url, then the test can enter data in input fields, click links and buttons and finally assert the result of the previous actions. </p>
<h3>CORS</h3>
<p>The first problem is <a href="http://en.wikipedia.org/wiki/Cross-origin_resource_sharing">Cross origin requests</a> - the tests need to be in the same domain as the application to be able to manipulate it.<br />
There are (at least) two possible solutions:</p>
<ul>
<li>run the application with the proper Access-Control-Allow-Origin header value</li>
<li>create a new application that merges the tests in the original application</li>
</ul>
<p>The first approach is too obtrusive. It makes the application aware of its clients (the tests).<br />
I prefer the second solution - by using <a href="http://maven.apache.org/plugins/maven-war-plugin/overlays.html">Maven War Overlays</a> we can create a Maven project that contains the tests and injects the original application in itself. This way the tests are available in my-tests folder in the web context for example.</p>
<h3>QUnit Setup</h3>
<p>To create QUnit tests you need to start with their HTML template. For our functional tests we need to put our additional iframe in its body. It is good to make the iframe big enough to be able to see what happens when you debug a test. In non-debug mode everything happens so fast that you cannot see anything ;-)</p>
<p>Here is the HTML template:</p>

{% highlight html %} 

<!DOCTYPE html>
<html>

<head>
	<title>Wicket Examples Functional tests</title>
	<meta http-equiv="content-type" content="text/html; charset=UTF-8">
	<link rel="stylesheet" href="lib/qunit-1.10.0.css" type="text/css" media="screen" />
    <script type="text/javascript" src="lib/jquery.min.js"></script>
    <script type="text/javascript" charset="utf-8">
        $q = jQuery.noConflict(true),
                $ = null,
                jQuery = null;
    </script>
    <script type="text/javascript" src="lib/qunit-1.10.0.js"></script>

	<!-- common helpers -->
    <script type="text/javascript" src="Helpers.js"></script>

	<!-- the modules under test -->
    <script type="text/javascript" src="tests/helloworld.js"></script>
    <script type="text/javascript" src="tests/echo.js"></script>
    <script type="text/javascript" src="tests/forminput.js"></script>
    
    <script type="text/javascript" src="tests/ajax/form.js"></script>
</head>

<body>
	<h1 id="qunit-header">Wicket Examples JS tests</h1>

	<h2 id="qunit-banner"></h2>

	<div id="qunit-testrunner-toolbar"></div>

	<h2 id="qunit-userAgent"></h2>

    <!-- Show the iframe to see how the tests run -->
    <iframe id="applicationFrame" width="100%" height="500px" src="../"></iframe>

    <ol id="qunit-tests"></ol>

    <div id="qunit-fixture">

    </div>
</body>
</html>

{% endhighlight %}

<p>The interesting things here are: </p>
<ul>
<li>we preserve current page"s (the tests" page) jQuery in $q variable and unset the original jQuery and $ variables just to avoid name clashes later in our tests.</li>
<li>we add Helpers.js which is a set of helper functions which are used by all our tests</li>
<li>we add &lt;script&gt; for all actual tests</li>
<li>and finally we add the iframe that will run the application</li>
</ul>
<p>And here is how the tests themselves look like:</p>
<p>helloworld.js:</p>

{% highlight javascript %}

$q(document).ready(function() {
	"use strict";

	module("Hello World");

	asyncTest("hello world", function () {
		expect(2);

		onPageLoad(function($) {

			var $message = $("#message");
			equal($message.length, 1, "The greeting is there");
			equal($message.text(), "Hello World!", "The greeting is correct")
			
			start();
		});
		getIframe().attr("src", "/helloworld");
	});

});

{% endhighlight %}

<p>The idea is that we use QUnit <a href="http://api.qunitjs.com/asyncTest/">asyncronous tests</a> because often our tests will need to go over several pages in the application and we should not allow QUnit to go to the next test before the current test is finished.</p>
<p>So what does this test do ?<br />
It registers a callback for the <em>load</em> event for the iframe and tells the iframe to go to the page we need to test. When the callback fires we make our assertions and then notify QUnit that we are ready with <a href="http://api.qunitjs.com/start/">QUnit#start()</a>. We can register several nested onPageLoad callbacks if the test needs to go over several pages.</p>
<p>The Ajax tests work similarly just they listen for Wicket"s Global . See <em>ajax/form.js</em> from the demo application for example.</p>
<h2>Demo</h2>
<p>A demo application that integrates wicket-examples.war can be found at <a href="https://github.com/martin-g/blogs/tree/master/functional-qunit">my GitHub account</a>.<br />
To run it:</p>
<ul>
<li>git clone git@github.com:martin-g/blogs.git</li>
<li>mvn install</li>
<li>cd functional-qunit</li>
<li>mvn jetty:run</li>
<li>in a browser open <a href="http://localhost:8080/js-test/all.html">http://localhost:8080/js-test/all.html</a>
</ul>
<p>The tests can be integrated in Continious Integration setup easily by using PhantomJS and/or Grunt.js (also based on PhantomJS).<br />
To run the tests with Grunt.js check the header content of grunt.js in the project.</p>
<h2>Credits</h2>
<p>The original idea is borrowed from <a href="http://michelgotta.posterous.com/user-interfaces-and-unittesting-with-qunit-an">User interfaces and unit testing with QUnit</a></p>
