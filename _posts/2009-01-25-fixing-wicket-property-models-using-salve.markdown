---
layout: post
status: publish
published: true
title: Fixing Wicket Property Models Using Salve
author: ivaynberg
author_login: ivaynberg
author_email: igor.vaynberg@gmail.com
excerpt: "<h3>Introduction</h3>\r\nIronically, property models are the most convenient
  and the most troublesome feature of wicket.  Lets use the following code as an example:\r\n<pre
  lang=\"java\">\r\nnew TextField(“zip”, \r\n    new PropertyModel(person, “addressBook.addresses.0.zip”));\r\n</pre>\r\n<p>\r\nThey
  are convenient because they make it trivial to bind components to domain objects
  using one line of code.\r\n</p>\r\n<p>\r\nThey are troublesome because they create
  code that is easily broken and whose breakage is not apparent until run time. For
  example: if the <code>AddressBook#getAddresses()</code> method used in the example
  above is renamed to <code>AddressBook#getLocations()</code> it will not cause a
  compilation error, but rather a runtime error when the page is rendered. This is
  a big problem unless every page of the webapp is covered by a unit test.\r\n</p>\r\n<h3>Possible
  Solutions</h3>\r\n"
wordpress_id: 338
wordpress_url: http://wicketinaction.com/?p=338
date: '2009-01-25 10:40:10 +0100'
date_gmt: '2009-01-25 08:40:10 +0100'
categories:
- Wicket
tags:
- Wicket
- models
- salve
- property
- propertymodel
- propertymodels
- model
comments:
- id: 399
  author: Daniele
  author_email: ildella@gmail.com
  author_url: http://lazydev.tumblr.com/
  date: '2009-01-27 17:37:10 +0100'
  date_gmt: '2009-01-27 15:37:10 +0100'
  content: 'Nice idea, I will give this a try, just one observation: is not possible
    for the bytecode analyzer to get the Class from modelObject.getClass() instead
    of specifying the class?'
- id: 400
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-01-27 17:49:36 +0100'
  date_gmt: '2009-01-27 15:49:36 +0100'
  content: '@Daniele: doing so will push the check back into runtime which is highly
    undesirable. All the analyzer sees is "new Pe(...)" and it checks just that part
    of the bytecode.'
- id: 428
  author: Antoine
  author_email: antoine.van.wel@gmail.com
  author_url: ''
  date: '2009-02-21 07:11:06 +0100'
  date_gmt: '2009-02-21 05:11:06 +0100'
  content: "Igor, \r\n\r\nI like your idea. I would definitely prefer to use such
    a PeModel over a PropertyModel (although I find the names Pe and PeModel too cryptic).
    Could it be taken a step further such that this new model would be integrated
    into the Wicket core, and that testing is happening automatically when running
    in development mode? \r\n\r\n--Antoine"
- id: 431
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-02-21 20:32:38 +0100'
  date_gmt: '2009-02-21 18:32:38 +0100'
  content: "@Anotine:\r\n\r\nThe names are a bit cryptic, but the reason is that I
    want to keep them as short as possible, I got tired of typing out PropertyModel
    all the time :)\r\n\r\nThere is no need to do this in development mode; when your
    application is running, in any mode, you will already get a runtime exception.
    The entire point of this is to shift the error from runtime into a lower phase
    such as compile or test because runtime is the hardest to test."
- id: 962
  author: bert bruynooghe
  author_email: bert.bruynooghe@gmail.com
  author_url: ''
  date: '2011-05-11 09:49:10 +0200'
  date_gmt: '2011-05-11 07:49:10 +0200'
  content: Based on this idea, I developped my own VerifiablePropertyModel. You can
    find some explanation on https://github.com/bertBruynooghe/wicket-verifiable/wiki
- id: 964
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2011-05-11 17:20:43 +0200'
  date_gmt: '2011-05-11 15:20:43 +0200'
  content: "@bert: this is my latest attempt at fixing this: https://github.com/42Lines/metagen\r\n\r\nits
    being used in production and works very well"
- id: 1051
  author: Carlos
  author_email: infiniter@gmail.com
  author_url: ''
  date: '2011-06-22 04:45:23 +0200'
  date_gmt: '2011-06-22 02:45:23 +0200'
  content: So, is this something you recommend using?
- id: 1052
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2011-06-22 06:11:44 +0200'
  date_gmt: '2011-06-22 04:11:44 +0200'
  content: |-
    currently i would recommend using this: https://github.com/42Lines/metagen
    We are using it in production with good results.
- id: 1060
  author: Marcue
  author_email: dragondickheadscontactmehere@gmail.com
  author_url: ''
  date: '2011-06-26 08:12:15 +0200'
  date_gmt: '2011-06-26 06:12:15 +0200'
  content: "Hello ivaynberg,\r\n\r\nI read you're commment and I have a lot of interest.\r\nI
    tried install metagen by adding this on my pom.xml:\r\n\r\n1.0-SNAPSHOT\r\n\r\nand...\r\n\r\n\r\nnet.ftlines.metagen\r\nmetagen-core\r\n${metagen.version}\r\n\r\n\r\nnet.ftlines.metagen\r\nmetagen-processor\r\n${metagen.version}\r\n\r\n\r\nnet.ftlines.metagen\r\nmetagen-wicket\r\n${metagen.version}\r\n\r\n\r\nbut
    Maven says: Missing artifacts for all of them.\r\n\r\nIs not working?\r\nOr is
    it wrong?"
- id: 1061
  author: Marcue
  author_email: dragondickheadscontactmehere@gmail.com
  author_url: ''
  date: '2011-06-26 08:13:35 +0200'
  date_gmt: '2011-06-26 06:13:35 +0200'
  content: "Oh my taggings did not show propertly in my comment.\r\nBut I did it exactly
    as shown at \r\nhttps://github.com/42Lines/metagen"
- id: 1062
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2011-06-26 08:49:59 +0200'
  date_gmt: '2011-06-26 06:49:59 +0200'
  content: |-
    @Marcue, metagen is not yet available in the maven central repo. that means you have to install it locally. check out the source and run
    <code>mvn install</code>
    this will deploy the necessary artifacts into your local maven repo, after which your project should find them.
---
<h3>Introduction</h3>
<p>Ironically, property models are the most convenient and the most troublesome feature of wicket.  Lets use the following code as an example:</p>

{% highlight java %}
new TextField(“zip”, 
    new PropertyModel(person, “addressBook.addresses.0.zip”));
{% endhighlight %}

<p>
They are convenient because they make it trivial to bind components to domain objects using one line of code.</p>
<p>
They are troublesome because they create code that is easily broken and whose breakage is not apparent until run time. For example: if the <code>AddressBook#getAddresses()</code> method used in the example above is renamed to <code>AddressBook#getLocations()</code> it will not cause a compilation error, but rather a runtime error when the page is rendered. This is a big problem unless every page of the webapp is covered by a unit test.</p>
<h3>Possible Solutions</h3>
<p><a id="more"></a><a id="more-338"></a></p>
<p>
There are a couple of ways to fix the problem:</p>
<ul>
<li>Do not use property models; instead create models that implement get/setObject methods using plain java code. Advantages: easy, refactor-safe, compile time errors. Disadvantage: extremely verbose, no default null handling in intermediate return values.
</li>
<li>Use proxies to generate dynamic builders. Advantages: refactor-safe, compile time errors. Disadvantages: runtime overhead (negligible), use of proxies limits code features(cannot intercept final methods, etc), most likely cannot be inlined thus requires at least two lines of code instead of one.
</li>
<li>Use a source code analyzer to check the expression string at compile time. Advantages: compile time errors, most visibility into code – can take advantage of generic types that are erased at runtime. Disadvantages: not refactor-safe, requires source for all classes used in the expression, extra compilation step.
</li>
<li>Use <code>apt</code> to generate metadata about classes and use that metadata instead of the expression string. Advantages: transparent code generation in java 6, compile time errors. Disadvantages: not refactor-safe, only works for classes compiled with apt processor.
</li>
<li>Use a bytecode analyzer to check the expression string at compile time or test time. Advantages: compile or test time errors, no extra compilation step if used in test time. Disadvantages: not refactor-safe.</li>
</ul>
<h3>Salve's solution</h3>
<p>
The bytecode analyzer is what I chose to implement as part of <a href="http://salve.googlecode.com" target="__blank">Salve</a>. It is implemented as a unit test that scans for use of <code>salve.expr.Pe</code> class that represents property expressions in Salve.</p>
<p>
A property model usage with Salve’s Pe class can look something like this:</p>

{% highlight java %}
new TextField(“zip”, 
    new PropertyModel(person,
        new Pe(Person.class, “addressBook.addresses.0.zip”).toString()));
{% endhighlight %}

<p>
While the unit test can be installed by creating a simple subclass of Salve’s <code>PropertyExpressionTest</code> within the project being checked:</p>

{% highlight java %}
public class ExpressionValidatorTest extends PropertyExpressionTest {}
{% endhighlight %}

<p>
When executed the unit test will scan for .class files in the project folder, look for instantiations of <code>Pe</code> class and check the string expression. Any error will cause the test to fail.</p>
<p>
Using <code>Pe</code> class does make the code somewhat longer. The checker supports definitions of custom classes. For example we can create a more convenient version of the PropertyModel like so:</p>

{% highlight java %}
public class PeModel<T> extends PropertyModel<T>
{
    public PeModel(Object modelObject, Class< ? > clazz, String expression)
    {
        // we do not use the clazz param, it is only here for bytecode analyzer
        super(modelObject, expression);
    }
}
{% endhighlight %}

<p>With usage:</p>

{% highlight java %}
new TextField(“zip”, 
    new PeModel(person, Person.class, “addressBook.addresses.0.zip”));
{% endhighlight %}

<p>And let the unit test know about it by specifying a matching rule:</p>

{% highlight java %}
public class ExpressionValidatorTest extends PropertyExpressionTest
{
   
    @Override
    protected Set<Rule> getRules()
    {
        Set<Rule> rules = super.getRules();
       rules.add(new Rule("commons/web/util/model/PeModel", Part.TYPE, Part.PATH));
        return rules;
    }
}
{% endhighlight %}

<p>
Rules match on the last x parameters. In the above example the rule will match instantiations of the <code>PeModel</code> class with constructor signature of <code>(*,Class rootType, String path)</code>.</p>
<p>
Another convenient usage pattern is binding components to fields of the parent via: <code>new PropertyModel(this, “name”);</code>. Salve’s checker supports this pattern like so:</p>

{% highlight java %}
    public PeModel(Object modelObject, String expression)
    {
        super(modelObject, expression);
    }

    rules.add(new Rule("commons/web/util/model/PeModel", Part.THIS, Part.PATH));
{% endhighlight %}

<h3>Installing Salve:</h3>

{% highlight bash %}
svn checkout http://salve.googlecode.com/svn/trunk/ salve
cd salve
mvn install
{% endhighlight %}

{% highlight xml %}
&lt;dependency&gt;
  &lt;groupId&gt;salve&lt;/groupId&gt;
  &lt;artifactId&gt;salve-expr-validator&lt;/artifactId&gt;
  &lt;version&gt;0.10-SNAPSHOT&lt;/version&gt;
&lt;/dependency&gt;
{% endhighlight %}

<h3>Conclusion</h3>
<p>The code to all this is still brand new, raw, unfinished, undocumented, untested, etc. This is mainly a call out for feedback and ideas. Let me know what you think.</p>
