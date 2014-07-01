---
layout: post
status: publish
published: true
title: Build and watch resources with Gulp.js
author: martin-g
date: '2014-07-01 10:00:00 +0300'
date_gmt: '2014-07-01 08:00:00 +0100'
categories:
- Wicket
tags: [gulp.js, livereload, node.js]
comments: []
---

# History 

Lately I observe that the <a target="_blank" href="http://nodejs.org/">Node.js</a> ecosystem provides much more (and better!) tools
for building web applications. For example there are tools to generate CSS like <a target="_blank" href="http://lesscss.org">Less.js</a>,
<a target="_blank" href="http://sass-lang.com">SASS</a>, <a target="_blank" href="http://learnboost.github.io/stylus/">Stylus</a>, minimizers like 
<a target="_blank" href="https://github.com/GoalSmashers/clean-css">Clean CSS</a>, <a target="_blank" href="http://lisperator.net/uglifyjs/">Uglify JS</a>,
linting tools like <a target="_blank" href="http://www.jshint.com/">JSHint</a> and <a target="_blank" href="http://www.jslint.com/">JSLint</a> and many more.
All these are available in the JVM world via <a target="_blank" href="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino">Rhino</a>
but it is kind of slow... Maybe it will get better with <a target="_blank" href="http://en.wikipedia.org/wiki/Nashorn_(JavaScript_engine)">Nashorn</a>
with Java 8 but it will take some time. And someone, like <a target="_blank" href="https://github.com/alexo/wro4j">Wro4j</a>, will have to create
adapters for all these useful tools.

# The solution

In my opinion it is much better to utilize the above tools directly with Node.js with its build tools like 
<a target="_blank" href="http://gulpjs.com/">Gulp</a>, <a target="_blank" href="http://gruntjs.com/">Grunt</a>, <a target="_blank" href="http://gearjs.org/">Gear</a>, etc.

To accomplish that the most easier way I've found is by using <a target="_blank" href="https://github.com/eirslett/frontend-maven-plugin">maven-frontend-plugin</a>
- a Maven plugin that downloads Node.js for you and integrates with Gulp and Grunt.

**Note**: there is a Gradle plugin in the works by the same author!

## Demo application

At <a target="_blank" href="https://github.com/martin-g/blogs/tree/master/wicket-nodejs-build">my GitHub account</a> you may find a demo application that uses Less 
to <a target="_blank" href="https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/gulpfile.js#L21">create</a> the CSS resources
and JSHint and UglifyJS to <a target="_blank" href="https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/gulpfile.js#L50">lint and minimize</a> 
the JavaScript resources.
At <a target="_blank" href="https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/pom.xml#L116">pom.xml</a> we use *maven-frontend-plugin* to execute
the default Gulp task that cleans and builds the final CSS and JS resources. The usage is as simple as *mvn clean compile*. At Maven's
*generate-sources* phase maven-frontend-plugin will execute the Gulp's tasks. 

**Note**: it will download Node.js and all needed Gulp plugins the first time and save them at *./node/* and *./node_modules/* folders. Make sure to ignore them in your SCM tool!

The usage in Wicket components is as you probably do it now:

{% highlight java %}
  response.render(CssHeaderItem.forReference(new CssResourceReference(HomePage.class, "res/demo.css")));
{% endhighlight %}

In *src/main/resources/com/mycompany/res* folder there is *demo.less* resource but Gulp's *stylesInPackages* task generates *demo.css*
and puts it in the classpath, so everything Just Works&trade;.

# Bonus

Most of the Node.js build work flows provide a way to watch the resources for modifications and reload the browser automatically
for you to see the changes. In the demo application we define <a target="_blank" href="https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/gulpfile.js#L90">watch</a>
 task that listens for changes in the Less and JavaScript resources and executes the earlier tasks automatically. Additionally
we make use of <a target="_blank" href="https://github.com/davidB/livereload-jvm">livereload-jvm</a> at the 
<a target="_blank" href="https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/src/test/java/com/mycompany/Start.java#L79">server side</a>
that can notify browser <a target="_blank" href="http://feedback.livereload.com/knowledgebase/articles/86242-how-do-i-install-and-use-the-browser-extensions-">plugins</a>
which will reload the page.
To try it:
<ol>
    <li>install the plugin for your preferred browser</li>
    <li>start the application via Start.java (starts embedded Jetty server and LiveReload-JVM server)</li>
    <li>open http://localhost:8080 in the browser</li>
    <li>open demo.less in your IDE and modify the background color to any valid color. Save the change.</li>
    <li>the browser will reload automatically !</li>
</ol>


# Conclusion

I have played with all this on Linux. I'm pretty sure it will work flawlessly on Mac too. People say that Node.js support for Windows has become
pretty good these days so it should work there too.

I hope you find all this useful. Since recently Wicket itself uses the solution described above to execute its 
<a target="_blank" href="https://github.com/apache/wicket/tree/master/testing/wicket-js-tests">JavaScript unit tests</a>.
