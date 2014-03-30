---
layout: post
status: publish
published: true
title: Wicket in Action Makeover
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
date: '2014-03-30 21:40:00 +0100'
categories:
- Wicket
tags: []
comments: []
---

A couple of days ago the Wicket in Action website was unavailable
without any notice. I'd like to explain why that was necessary and what
I have done to prevent such outages in the future. This outage also
presented an opportunity to work on the design of the Wicket in Action
website.

### Wicket in Action: hosted by Wordpress

Wicket in Action was powered by a self-hosted [Wordpress][2]
installation since the beginning of this blog in 2008.

So why not use Wicket to create a blogging engine? I always figured
that it was more important to write content, not the backend software.
Hosting a Wordpress blog is almost gratis, where Java hosting brought
considerable costs with it. The choice for Wordpress was quite simple
at that time.

Wordpress has been a great blogging platform that got better with each
release. Most notably the automatic updates were a boon and the admin
UI was evolving nicely. With a press of a button I could upgrade my
blogs to a new major release of Wordpress. And in the latest releases
they even upgraded Wordpress for you automatically to patch releases.

### Saying goodbye to Wordpress

Unfortunately Wordpress has been the target of malicious software,
hackers and automated scripts. When you have to fully nuke and
reinstall because a new exploit has crippled your site it is time part
ways.

This caused quite a bit of a maintenance hassle, and a nagging feeling
that at any given moment the website could be used to distribute
malware or harvest information of blog visitors. A couple of months ago
the website suddenly was blocked by google for distributing malware,
causing visitors to see a big warning screen that the website could
harm their computers.

The final straw that pushed me to turn off Wordpress was that I noticed
in my access logs some weird pages that really had nothing to do with
Wicket:

* /2014/03/crack-sam-broadcaster-pro/
* /2014/03/crack-wondershare-pdf-editor-or-serial-number/

These pages did not turn up in the Wordpress administration UI so it
was difficult to spot them.

I immediately shutdown this website and my [own personal blog][1]. I
have put a notice in place but somehow my hosting provider shows a 'not
registered' page instead.

Goodbye Wordpress, I will not miss you.

### Say hello to jekyll

The first thing I wanted to achieve was being free of active executing
code on my hosting server. This led me to turning this website into
serving only static markup.

#### Faster, more secure, better scalability

A static web site is safer than running Wordpress. There's nothing to
execute on the server! All the pages are pre-rendered and ready to be
sent to browsers without any code executing. There are no accounts to
break: all editing is done offline.

A static website is also much faster to serve by web servers. The web
server only has to copy a file from disk to a network socket. There is
no code execution inbetween: all pages are pre-rendered and nothing
needs to be updated dynamically. This makes it easy to cache each page:
in a file cache or in the HTTP server.

Having only static markup files makes it rather easy for a HTTP server
to serve thousands of requests. Here's a list Wordpress has to perform
to serve a blog post:

 * start a PHP process
 * establish a connection to the database
 * query the database to retrieve the post
 * query the datebase to retrieve the author
 * query the database to retrieve the comments
 * query the database to retrieve active plugins
 * render the markup whilst executing any number of plugins and themes
 * close the connection to the database

I am sure I have missed quite a few steps in this list. While only a
single step, there's no saying what a given plugin and theme in
Wordpress can do. 

In the new, static content situation the HTTP server has the following
tasks to perform:

 * read file from disk
 * send file to network

This is more concise and a lot easier to execute for a HTTP server.

One of the telltale indicators that a website is running a CMS
(Wordpress) is when a highly trafficed site (for example Reddit,
HackerNews, Slashdot, Daringfireball or Twitter) features the site on
the front page, the server will crumble under the amount of incoming
requests. Typically the first thing to give up is the database: with a
couple of hundred concurrent requests it is bound to consume too much
memory and the server will typically give up.

Wicket in action has not received that much traffic but you never know
who will link to your blog post. Give a positive review of JEE 7 or
Java 8 and suddenly you have thousands of Java developers looking at
your post.

While not strictly necessary from a performance standpoint, moving to a
static website makes sense.

#### Jekyll as the powering engine

There were a couple of factors to choose [Jekyll][3] as the platform
for Wicket in Action and my [other website][1]:

1. well known technology
2. servable from github.com
3. automatic migration from Wordpress

The [Apache Wicket][4] project uses Jekyll to generate [the
website][5]. As such the Wicket committers have ample experience with
Jekyll. The tooling is well understood and there's ample documentation
available.

Secondly github.com can serve Jekyll generated websites directly. You
create a project and push your Jekyll content to github. A few moments
later Github publishes the content. Github uses a <abbr title="Content
Delivery Network">CDN</abbr> for the static websites. This gives our
blog a global presence and speedy delivery.

I did not want to loose 6 years of content, so being able to import the
Wordpress database and have all content available was a big plus.

I was able to replicate the URL schema of the Wordpress installation,
so all links pointing to articles will still work (no broken web)!

#### A new design

At first the Wicket in Action website had a nice design that followed
the book closely, but was tailored for a blog:

<img alt="Screenshot of original Design of Wicket in Action" 
     src="/img/2014-03-30-wia-orig-design.png" 
	 width="512" />

After a couple of Wordpress upgrades my custom theme did not work
anymore. So I had to either reimplement my custom theme, or just pick a
default theme. I chose the latter and Wicket in Action has used a
[plain design][6] for some time. I was never happy with that.

The migration to Jekyll also brought a really hidious default design,
so I had to do some design work.

The goals I set myself were:

 * stay close to the Wicket in Action book (use the smoking person, use
   the book color)
 * few menu items visible
 * clear, easy to read content
 * enough room for content and code examples

I wanted a content area that was wider than the original design to
accomodate code better. I went into Keynote (my 'design' application)
for the initial sketch and after letting it sit for a day started to
implement it in the Jekyll site.

The result is what you see before you. It is not too snazzy, nor too
fashionable.

A couple of things I have yet to address or implement:

 * author names instead of accounts
 * metadata clean up
 * code blocks design: better font and font-size
 * typography (font sizes of headings)
 * responsive UI (legibility on small devices)
 * search functionality
 * tags and categories (though who uses those anyway?)
 * reimplement comments
 * migrate all content from HTML to markdown

This new setup gives Wicket in Action (and my other blog) a good solid
foundation for the coming years. I hope you like the new design and
that new content keeps on flowing!

[1]: http://martijndashorst.com
[2]: http://wordpress.org
[3]: http://jekyllrb.com
[4]: http://wicket.apache.org
[5]: https://svn.apache.org/repos/asf/wicket/common/site/trunk/
[6]: https://web.archive.org/web/20130704083056/http://wicketinaction.com/
