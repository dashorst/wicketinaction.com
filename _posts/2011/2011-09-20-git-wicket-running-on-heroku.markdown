---
layout: post
status: publish
published: true
title: Git Wicket running on Heroku
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 656
wordpress_url: http://wicketinaction.com/?p=656
date: '2011-09-20 10:00:03 +0200'
date_gmt: '2011-09-20 08:00:03 +0200'
categories:
- Wicket
tags:
- Heroku
comments:
- id: 1119
  author: James Ward
  author_email: james@jamesward.org
  author_url: http://www.jamesward.com
  date: '2011-09-20 10:54:43 +0200'
  date_gmt: '2011-09-20 08:54:43 +0200'
  content: "Awesome stuff!  Thanks for posting this.  One small correction...  On
    the Cedar stack the file system is writeable but ephemeral - so it can go away
    at any time.  External / persistent storage (like Amazon S3) should be used for
    files that must be durable.\r\n\r\n-James"
- id: 1120
  author: dashorst
  author_email: martijn.dashorst@gmail.com
  author_url: http://martijndashorst.com
  date: '2011-09-20 11:16:38 +0200'
  date_gmt: '2011-09-20 09:16:38 +0200'
  content: I amended the post with your comment, thanks!
- id: 1168
  author: Wicket 6 + CDI on HerokuThere is no place like ::1
  author_email: ''
  author_url: http://www.bloggure.info/work/wicket-6-cdi-on-heroku.html
  date: '2013-03-04 15:04:21 +0100'
  date_gmt: '2013-03-04 13:04:21 +0100'
  content: '[...] initial investigation led me to this blog post from Martijn Dashorst explaining
    how to deploy a Wicket 1.5 application to Heroku, the service has slightly evolved
    [...]'
---
<p>At <a href="http://vimeo.com/28791557">JavaZone 2011</a> I met with <a href="http://jamesward.com">James Ward</a>, technology evangelist for <a href="http://heroku.com">Heroku</a> and had a brief discussion on what it would take to run Wicket applications on Heroku. We figured that it wouldn't be too hard to get a Wicket application running on a single instance (a dyno in Heroku lingo).</p>
<p>So I tasked myself yesterday evening to get a Wicket quickstart running on Heroku. Basically the recipe is as follows:<br />
 - generate a quickstart<br />
 - modify the pom such that the packaging goes from <code>war</code> to <code>jar</code><br />
 - modify the pom to depend on jetty at compile time, and on the servlet api on compile time (not provided)<br />
 - modify the pom to generate a shell script to start the application (using <a href="http://mojo.codehaus.org/appassembler/appassembler-maven-plugin/">appassembler-maven-plugin</a>)<br />
 - add a Procfile to the root of your project<br />
 - add the following <code>Start</code> class to the <code>src/main/java</code> path</p>

{% highlight java %}
import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.webapp.WebAppContext;

public class Start {
    public static void main(String[] args) throws Exception {
        String webPort = System.getenv("PORT");
        if (webPort == null || webPort.isEmpty()) {
            webPort = "8080";
        }
        String webappDirLocation = "src/main/webapp/";

        Server server = new Server(Integer.valueOf(webPort));
        WebAppContext root = new WebAppContext();
        root.setContextPath("/");
        root.setDescriptor(webappDirLocation + "/WEB-INF/web.xml");
        root.setResourceBase(webappDirLocation);
        root.setParentLoaderPriority(true);

        server.setHandler(root);
        server.start();
        server.join();
    }
}
{% endhighlight %}

<p>For some reason the <code>Start</code> class provided by the quickstart archetype doesn't work on Heroku, but the above class is simple enough. It should be included in the <code>src/main/java</code> folder (and default package) because with Heroku, you bring your own server (in our case the Jetty server). The code will pick up the provided socket port to connect to (Heroku supplies external settings using environment variables) and start up the Jetty server.</p>
<p>The <code>Procfile</code> is just a descriptor for Heroku to identify the start up script.</p>

{% highlight bash %}
web:   sh target/bin/webapp
{% endhighlight %}

<p>When you have everything up and running locally:</p>

{% highlight bash %}
$ mvn install
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building Heroku Wicket Quick Start 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
...

$ export REPO=~/.m2/repository
$ sh target/bin/webapp
INFO  - log                        - jetty-7.3.0.v20110203
...
INFO  - WebApplication             - [wicket.heroku-wicket] Started Wicket version 1.5.0 in DEVELOPMENT mode
********************************************************************
*** WARNING: Wicket is running in DEVELOPMENT mode.              ***
***                               ^^^^^^^^^^^                    ***
*** Do NOT deploy to your live server(s) without changing this.  ***
*** See Application#getConfigurationType() for more information. ***
********************************************************************
INFO  - log                        - Started SelectChannelConnector@0.0.0.0:8080
{% endhighlight %}

<p>When you see this (don't forget to add the <code>REPO</code> environment variable to your shell and point it to your maven repository), you can push your code to Heroku using</p>

{% highlight bash %}
heroku create -s cedar
git push heroku master
{% endhighlight %}

<p>Getting the Wicket examples running on Heroku was easy now that the recipe is clear. It just works™.</p>
<p>You can see the code modifications I made on my github repositories for both the <a href="https://github.com/dashorst/heroku-wicket-quickstart">quickstart</a> and the <a href="https://github.com/dashorst/heroku-wicket-examples">wicket-examples</a>.</p>
<p>You can see the <a href="http://wicket-quickstart.herokuapp.com">Heroku QuickStart in action</a>, as well as the <a href="http://wicket-examples.herokuapp.com">Heroku Wicket Examples</a> both running on a single dyno.</p>
<h2>Things worthy of note</h2>
<p>Not everything is rosy in the garden of Heroku and Wicket.</p>
<h3>Server side state</h3>
<p>Wicket uses the HTTP session to communicate the last accessed page to a cluster. As HTTP session clustering is not supported on Heroku (unless you persist the session in a memcache or redis instance), you will loose running sessions when Heroku reassigns your dyno. Of course you can persist your HTTP session in a memcache or redis database.</p>
<p>Since Wicket stores the component tree in the filesystem (by default, you can store it in a memcache or other cloud storage) and Heroku has a <a href="http://devcenter.heroku.com/articles/read-only-filesystem">read-only filesystem</a> the page store can be evaporated at any time. This will affect the back button support when Heroku has reclaimed your specific dyno instance.</p>
<p><b>Update:</b> As James Ward points out in the comments,<br />
<blockquote>the file system is writeable but ephemeral – so it can go away at any time</p></blockquote>
<p>.</p>
<h3>Concurrency support</h3>
<p>Heroku doesn't support sticky sessions, and as such concurrent access to the component tree cannot be prevented by synchronizing on the page. I think we might be able to come up with something that will work in such an environment in the future as we migrate towards more granular locks (Wicket 1.4 would lock on a page map, 1.5 on a page instance). So running a Wicket application on multiple dynos is not supported (yet).</p>
<p><del datetime="2011-09-20T21:01:38+00:00">One advantage of Heroku is that one dyno can only process one request at a time, so synchronization on the component tree in a single JVM is no longer necessary.</del></p>
<p><b>UPDATE:</b> After some discussion with the Heroku guys, it appears I based this assumption on old information on the Heroku website. The new Java supported platform called <i>cedar stack</i> does <a href="http://devcenter.heroku.com/articles/http-routing#the_herokuappcom_http_stack">support multiple requests coming in at the same time</a>. The running Wicket Heroku example applications are deployed to the herokuapp.com platform, and make use of the cedar stack.</p>
<h3>Conclusion</h3>
<p>It is possible to run Wicket applications on a Heroku instance which is a great testament of Heroku's engineering team. I found it easier to get a Wicket application up and running on Heroku than on Google App Engine. There are several loose ends, and we might not be able to fix those given Wicket's architecture. That said, I find Heroku's architecture quite compelling and am anxious to get Wicket fully supported on this platform.</p>
