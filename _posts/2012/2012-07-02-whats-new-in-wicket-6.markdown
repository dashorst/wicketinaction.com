---
layout: post
status: publish
published: true
title: What's new in Wicket 6
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 745
wordpress_url: http://wicketinaction.com/?p=745
date: '2012-07-02 15:10:20 +0200'
date_gmt: '2012-07-02 13:10:20 +0200'
categories:
- Releases
- Wicket
tags: []
comments: []
---
<h1>What's new in Wicket 6</h1>
<p>Wicket 6.0 is around the corner and it is time to describe what's new and what's has changed since Wicket 1.5. This article will mention briefly the major things. More details about the bigger changes will be provided in separate articles.</p>
<h2>Why 6.0 ?</h2>
<p>The Wicket team decided to use semantic versioning (<a href="http://semver.org/">http://semver.org</a>) from now on. That means that having version like x.y.z we will fix bugs in 'z', introduce new features which do not break APIs in 'y' and break APIs in 'x' versions.<br />
This new version has APIs breaks here and there (more on this below) so it must be a change in 'x' part of the version.<br />
Wicket 2.0 has been used already few years ago, somewhere between Wicket 1.2 and 1.3 for a major change which didn't work well and has been discontinued. Next option was Wicket 3.0 but since Wicket 6.0 now requires JDK 6 we decided to jump to 6.0.0. That doesn't mean that Wicket 7 will require JDK 7.</p>
<h2>Major changes in Wicket 6.0</h2>
<h3>JavaScript uses jQuery</h3>
<p>One of the bigger changes is in the Ajax support. Since version 6.0 Wicket will use jQuery internally for its JavaScript needs. Initially we started with an implementation based on YUI but the communitity asked us to re-think this decision and use jQuery instead because most of Wicket's users use jQuery at the moment. So we switched to jQuery as backing library but made it such way that it is easy to replace the current implementation with another one based on another JavaScript library if this is needed by some of the users.</p>
<p>Another change in the this area is that now Wicket uses JavaScript event registration (e.g. jQuery(element).on('click', function() { ... }) instead of using inline attributes in the HTML (e.g. &lt;a onclick="..."&gt;).</a></p>
<h3>Resource management</h3>
<p><em title="org.apache.wicket.request.resource.ResourceReference">ResourceReference</em> class now can declare its dependencies so you just need to contribute a ResourceReference for your jQuery plugin for example, and the ResourceReference itself will tell Wicket that jQuery has to be delivered as well.</p>
<p>Additionally <em title="org.apache.wicket.request.resource.PackageResourceReference">PackageResourceReference</em> and its specializations can deliver minified version of their resource if such is available. For example by using<br />
'new JavaScriptResourceReference(MyComponent.class, "my.js")' Wicket will deliver my.js in development mode but will deliver my.min.js in production mode if it is available in the same folder.</p>
<h3>New Tree component</h3>
<p>The previous <em title="org.apache.wicket.extensions.markup.html.tree.Tree">Tree</em> component implementation used AWT/SWING APIs and it was not possible to use it in more restricted environments like Google AppEngine. That's why it has been deprecated and will be removed in a some next major release of Wicket.<br />
<em title="org.apache.wicket.extensions.markup.html.repeater.tree.AbstractTree">AbstractTree</em> and its specializations <em title="org.apache.wicket.extensions.markup.html.repeater.tree.NestedTree">NestedTree</em> and <em title="org.apache.wicket.extensions.markup.html.repeater.tree.TableTree">TableTree</em> are the replacements.</p>
<h2>New modules</h2>
<p>A few new modules have been added to the Wicket portfolio. For now they are with experimental status but depending on the adoption they will either join the other modules or will be removed.</p>
<h3>Integration with Atmosphere</h3>
<p>The first new module is Wicket Atmosphere (Maven id: org.apache.wicket:wicket-atmosphere:jar). It is an integration with <a href="https://github.com/Atmosphere/atmosphere/">Atmosphere</a> project and provides server push functionality. Depending on the configuration it may use WebSocket, long polling, streaming, JSONP, Server Side events mechanisms.</p>
<h3>Wicket Native WebSocket</h3>
<p>The second new module is again related to server push functionality but uses native integration with the underlying web container if such is supported. At the moment only Jetty 7+ and Tomcat 7+ are supported.</p>
<h2>What didn't make it</h2>
<h3>Component queueing</h3>
<p>The task of component queueing was to remove the requirement to keep in sync the component trees in the markup and in Java. Due to various technical problems this task has been postponed and may be implemented in a later version of Wicket.</p>
<h3>CDI integration</h3>
<p>The Contexts and Dependency injection (CDI) integration implemented in <a href="https://github.com/42Lines/wicket-cdi">wicket-cdi</a> cannot be merged to Apache Wicket for now because it uses some libraries which are not available yet in Maven central repository. Until this problem is resolved users still can use it as described at the project site.</p>
<h2>What else</h2>
<p>Some more information about the major changes can be found at the <a href="https://cwiki.apache.org/WICKET/wicket-60-roadmap.html">Roadmap</a> page and about other minor changes at the <a href="https://cwiki.apache.org/WICKET/migration-to-wicket-60.html">Migration page</a></p>
<p>The Wicket team!</p>
