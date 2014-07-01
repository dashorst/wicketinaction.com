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

Lately I observe that the (Node.js)[http://nodejs.org/] ecosystem provides much more (and better!) tools
for building web applications. For example there are tools to generate CSS like (Less.js)[http://lesscss.org/],
(SASS)[http://sass-lang.com/], (Stylus)[http://learnboost.github.io/stylus/], minimizers like 
(Clean CSS)[https://github.com/GoalSmashers/clean-css], (Uglify JS)[http://lisperator.net/uglifyjs/],
linting tools like (JSHint)[http://lisperator.net/uglifyjs/] and (JSLint)[http://www.jslint.com/] and many more.
All these are available in the JVM world via (Rhino)[https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino]
but it is kind of slow... Maybe it will get better with (Nashorn)[http://en.wikipedia.org/wiki/Nashorn_(JavaScript_engine)]
with Java 8 but it will take some time. And someone, like (Wro4j)[https://github.com/alexo/wro4j], will have to create
adapters for all these useful tools.

# The solution

In my opinion it is much better to utilize the above tools directly with Node.js with its build tools like 
(Gulp)[http://gulpjs.com/], (Grunt)[http://gruntjs.com/], (Gear)[http://gearjs.org/], etc.

To accomplish that the most easier way I've found is by using (maven-frontend-plugin)[https://github.com/eirslett/frontend-maven-plugin]
- a Maven plugin that downloads Node.js for you and integrates with Gulp and Grunt.
**Note**: there is a Gradle plugin in the works by the same author!

## Demo application

At https://github.com/martin-g/blogs/tree/master/wicket-nodejs-build you may find a demo application that uses Less 
(to create)[https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/gulpfile.js#L21] the CSS resources
and JSHint and UglifyJS (to lint and minimize)[https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/gulpfile.js#L50] 
the JavaScript resources.
At (pom.xml)[https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/pom.xml#L116] we use maven-frontend-plugin to execute
the default Gulp task that cleans and builds the final CSS and JS resources. The usage is as simple as `mvn clean compile`. At Maven's
*generate-sources* phase maven-frontend-plugin will execute the Gulp's tasks. **Note**: it will download Node.js and all needed Gulp
plugins the first time and save them at ./node/ and ./node_modules/ folders. Make sure to ignore them in your SCM tool!

The usage in Wicket components is as you probably do it now:

{% highlight java %}
  response.render(CssHeaderItem.forReference(new CssResourceReference(HomePage.class, "res/demo.css")));
{% endhighlight %}

In `src/main/resources/com/mycompany/res` there is `demo.less` resource but Gulp's 'stylesInPackages' task generates `demo.css`
and puts it in the classpath, so everything Just Works TM.

# Bonus

Most of the Node.js build work flows provide a way to watch the resources for modifications and reload the browser automatically
for you to see the changes. In the demo application we (define)[https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/gulpfile.js#L90]
a `watch` task that listens for changes in the Less and JavaScript resources and executes the earlier tasks automatically. Additionally
we make use of (livereload-jvm)[https://github.com/davidB/livereload-jvm] at the (server side)[https://github.com/martin-g/blogs/blob/master/wicket-nodejs-build/src/test/java/com/mycompany/Start.java#L79]
that can notify browser (plugins)[http://feedback.livereload.com/knowledgebase/articles/86242-how-do-i-install-and-use-the-browser-extensions-] 
which will reload the page.
To try it:
 - install the plugin for your preferred browser
 - start the application via Start.java (starts embedded Jetty server and LiveReload-JVM server)
 - open http://localhost:8080 in the browser
 - open demo.less in your IDE and modify the background color to any valid color. Save the change.
 - the browser will reload automatically !


# Conclusion

I have played with all this on Linux. I'm pretty sure it will work flawlessly on Mac too. People say that Node.js support for Windows has become
pretty good these days so it should work there too.

I hope you find all this useful. Since recently Wicket itself uses the solution described above to execute its 
(JavaScript unit tests)[https://github.com/apache/wicket/tree/master/testing/wicket-js-tests].
