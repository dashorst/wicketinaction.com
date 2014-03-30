---
layout: post
status: publish
published: true
title: Apache Wicket 1.4 takes type safety to the next level
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
excerpt: "The Apache Wicket project is proud to announce the release of <a href=\"http://wicket.apache.org\">Apache
  Wicket 1.4</a>. Apache Wicket is an open source, component oriented Java web application
  framework. With overwhelming support from the user community, this release marks
  a departure from the past where we leave Java 1.4 behind and we require Java 5 as
  the minimum JDK version. By moving to Java 5 as the required minimum platform, we
  were able to utilize Java 5 idioms and increase the type safety of our APIs. Using
  Java generics you can now write typesafe web applications and create typesafe, self
  documenting, reusable custom components.\r\n\r\n<h3>Download Apache Wicket 1.4</h3>\r\n\r\nYou
  can download the release here: <a href=\"http://www.apache.org/dyn/closer.cgi/wicket/1.4.0\">http://www.apache.org/dyn/closer.cgi/wicket/1.4.0</a>\r\n\r\nOr
  use this in your Maven pom's to upgrade to the new version:\r\n<pre lang=\"xml\"><dependency>\r\n
  \ <groupId>org.apache.wicket</groupId>\r\n  <artifactId>wicket</artifactId>\r\n
  \ <version>1.4.0</version>\r\n</dependency>{% endhighlight %}\r\nYou will need to upgrade <i>all</i>
  modules (i.e. wicket, wicket-extensions) to their 1.4 counterparts. It is not possible
  to mix Wicket 1.3 libraries with 1.4 libraries due to API changes.\r\n\r\n<h3>Most
  notable changes</h3>\r\n\r\nFrom all the changes that went into this release, the
  following are the most important ones:\r\n<ul>\r\n<li>Generified IModel interface
  and implementations increases type safety in your Wicket applications</li>\r\n<li>Component#getModel()
  and Component#setModel() have been renamed to getDefaultModel() and setDefaultModel()
  to better support generified models</li>\r\n<li>The Spring modules have been merged
  (wicket-spring-annot is now obsolete, all you need is wicket-spring)</li>\r\n<li>Many
  API's have been altered to better work with Java 5's idioms</li>\r\n<li>Wicket jars
  are now packaged with metadata that makes them OSGI bundles</li>\r\n</ul>Apart from
  these changes, the release is mostly compatible with Wicket 1.3 and upgrading shouldn't
  take too long. Early adopters report about a days work to upgrade medium to large
  applications to Wicket 1.4. Read the <a href=\"http://cwiki.apache.org/WICKET/migrate-14.html\">migration
  guide</a> to learn more about the changes in our APIs. To learn more about all the
  improvements and new features that went into this release, check the solved issue
  list in our <a href=\"https://issues.apache.org/jira/secure/IssueNavigator.jspa?mode=hide&requestId=12313364\">JIRA
  instance</a>.\r\n"
wordpress_id: 442
wordpress_url: http://wicketinaction.com/?p=442
date: '2009-07-30 12:55:30 +0200'
date_gmt: '2009-07-30 10:55:30 +0200'
categories:
- Releases
- Wicket
tags:
- Wicket
- Releases
comments:
- id: 560
  author: Jason Duke
  author_email: jduke@bluefishgroup.com
  author_url: http://www.bluefishgroup.com
  date: '2009-07-30 16:27:22 +0200'
  date_gmt: '2009-07-30 14:27:22 +0200'
  content: Hooray! Very exciting stuff, can't wait to try it out...
- id: 564
  author: Bruno Borges
  author_email: bruno.borges@gmail.com
  author_url: http://blog.brunoborges.com.br
  date: '2009-07-30 19:03:08 +0200'
  date_gmt: '2009-07-30 17:03:08 +0200'
  content: "Wicket 1.3 is dead, long live Wicket 1.4 !! :D\r\n\r\nGreat work guys!\r\n\r\nCheers,\r\nBruno"
- id: 565
  author: Rob Sonke
  author_email: rob@tigrou.nl
  author_url: http://tigrou.nl
  date: '2009-08-01 12:23:32 +0200'
  date_gmt: '2009-08-01 10:23:32 +0200'
  content: Congrats guys, we're using 1.4 for quite a while now and it's great!
- id: 570
  author: david_p
  author_email: david.rapin@gmail.com
  author_url: ''
  date: '2009-08-13 00:32:12 +0200'
  date_gmt: '2009-08-12 22:32:12 +0200'
  content: "i have been waiting for this one for a while! \nthank you very much for
    the hard work!\ni will migrate all my websites to 1.4 as soon as possible, and
    remove all those ugly casts :D\n\ndavid"
- id: 575
  author: Dane
  author_email: danelaverty@gmail.com
  author_url: http://marshcousins.wordpress.com
  date: '2009-08-18 01:13:56 +0200'
  date_gmt: '2009-08-17 23:13:56 +0200'
  content: |-
    You are amazing! Thanks for all your work!

    Also, incidentally, how do I view older posts on this site? Perhaps I'm blind, but I don't see any link to older posts at the bottom of the page...
- id: 602
  author: VA
  author_email: va1958@gmail.com
  author_url: http://valoanquestions.com/
  date: '2009-10-05 23:33:18 +0200'
  date_gmt: '2009-10-05 21:33:18 +0200'
  content: "i have been waiting for this one for a while! \nthank you very much for
    the hard work!\ni will migrate all my websites to 1.4 as soon as possible, and
    remove all those ugly casts :D\n\ndavid"
---
<p>The Apache Wicket project is proud to announce the release of <a href="http://wicket.apache.org">Apache Wicket 1.4</a>. Apache Wicket is an open source, component oriented Java web application framework. With overwhelming support from the user community, this release marks a departure from the past where we leave Java 1.4 behind and we require Java 5 as the minimum JDK version. By moving to Java 5 as the required minimum platform, we were able to utilize Java 5 idioms and increase the type safety of our APIs. Using Java generics you can now write typesafe web applications and create typesafe, self documenting, reusable custom components.</p>
<h3>Download Apache Wicket 1.4</h3>
<p>You can download the release here: <a href="http://www.apache.org/dyn/closer.cgi/wicket/1.4.0">http://www.apache.org/dyn/closer.cgi/wicket/1.4.0</a></p>
<p>Or use this in your Maven pom's to upgrade to the new version:</p>
{% highlight xml %}<dependency>
  <groupId>org.apache.wicket</groupId>
  <artifactId>wicket</artifactId>
  <version>1.4.0</version>
</dependency>{% endhighlight %}
<p>You will need to upgrade <i>all</i> modules (i.e. wicket, wicket-extensions) to their 1.4 counterparts. It is not possible to mix Wicket 1.3 libraries with 1.4 libraries due to API changes.</p>
<h3>Most notable changes</h3>
<p>From all the changes that went into this release, the following are the most important ones:</p>
<ul>
<li>Generified IModel interface and implementations increases type safety in your Wicket applications</li>
<li>Component#getModel() and Component#setModel() have been renamed to getDefaultModel() and setDefaultModel() to better support generified models</li>
<li>The Spring modules have been merged (wicket-spring-annot is now obsolete, all you need is wicket-spring)</li>
<li>Many API's have been altered to better work with Java 5's idioms</li>
<li>Wicket jars are now packaged with metadata that makes them OSGI bundles</li>
</ul>
<p>Apart from these changes, the release is mostly compatible with Wicket 1.3 and upgrading shouldn't take too long. Early adopters report about a days work to upgrade medium to large applications to Wicket 1.4. Read the <a href="http://cwiki.apache.org/WICKET/migrate-14.html">migration guide</a> to learn more about the changes in our APIs. To learn more about all the improvements and new features that went into this release, check the solved issue list in our <a href="https://issues.apache.org/jira/secure/IssueNavigator.jspa?mode=hide&requestId=12313364">JIRA instance</a>.<br />
<a id="more"></a><a id="more-442"></a></p>
<h3>Increased type safety</h3>
<p>Moving towards Java 5 has given us the opportunity to utilize generics and clarify our API's. For example, take a look at DropDownChoice—one of the components with the most questions on our list prior to 1.4. A DropDownChoice component is a form component that displays a list of available choices in a drop down box, and allows one selection to be made. DropDownChoice components are typically used to display a list of countries, nationalities, credit card processors, etc.</p>
<p>The signature of a constructor for the DropDownChoice component in Wicket 1.3 was:</p>
{% highlight java %}public class DropDownChoice extends ...
    public DropDownChoice(String id, IModel model, IModel choices)
}{% endhighlight %}
<p>As you can see, this constructor doesn't give much insight into what goes where (other than the names of the parameters). The first parameter is the component identifier, the second parameter is the model that contains the selection, and the third parameter is a model that contains the list of choices from which the user can select one. You'll have to read the JavaDoc to assign the right IModel values to the right parameters. Now take a look at the same constructor, but now in Wicket 1.4. The signature for our generified constructor looks like the following example.</p>
{% highlight java %}public <T> DropDownChoice extends ...
    public DropDownChoice(String id, IModel<T> model, IModel<? extends List<? extends T>> choices)
}{% endhighlight %}
<p>Here we communicate that the first IModel parameter is a T, which is the single value that will be provided when the DropDownChoice selects one value. The second parameter provides a List of objects that extend T, the choices from which to select one value. This was not apparent in the Wicket 1.3 API, and the type safety brought by generics make this much more clear, albeit much more verbose.</p>
<h3>Removal of default model from component</h3>
<p>In Wicket 1.3 each component had by default a model: a Label had a model, a Link and even WebMarkupContainer had a model property (all because they extend Component which has a model property). When we generified IModel, this had averse effects on Component: suddenly *all* components had to be generified and had to take the type parameter of the model that was associated with it. But that poses problems for components that either do not use a model or use two different model types: which one should be in the lead? We chose to generify only the components that clearly benefited from the extra type information, leading to clean code like this:</p>
{% highlight java %}ListView<Person> peopleListView = new ListView<Person>("people", people) {
        protected void populateItem(ListItem<Person> item) {
            item.add(new Link<Person>("editPerson", item.getModel()){
                public void onClick() {
                    Person p = getModelObject();
                    setResponsePage(new EditPersonPage(p));
                }
            });
        }
    };{% endhighlight %}
<h3>Support</h3>
<p>As always, the user lists are very active, and if you need help upgrading or just want to discuss this release, send an email to the users list.  More information about the community and mailing lists can be found at <a href="http://wicket.apache.org/community.html">http://wicket.apache.org/community.html</a></p>
