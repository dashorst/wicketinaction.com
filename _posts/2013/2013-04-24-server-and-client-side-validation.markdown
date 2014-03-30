---
layout: post
status: publish
published: true
title: Server and client side validation
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 841
wordpress_url: http://wicketinaction.com/?p=841
date: '2013-04-24 18:04:33 +0200'
date_gmt: '2013-04-24 16:04:33 +0200'
categories:
- Wicket
tags: []
comments: []
---
<p>Maybe not so well known feature in Wicket is that the form component <em title="org.apache.wicket.validation.IValidator">validator</em>s are actually <em title="org.apache.wicket.behavior.Behavior">Behavior</em>s, or if they are not then a special <em title="org.apache.wicket.validation.ValidatorAdapter">adapter</em> is used, so at the end they are stored as a behavior in the component.</p>
<p>Being a behavior gives the validator the chance to be notified when the respective form component is being rendered. And this makes it possible to make a validator that can validate at the server side and to contribute whatever is needed for some client side (JavaScript) library to make the same validation before the request is even made to the server.</p>
<p>At <a href="https://github.com/martin-g/blogs/tree/master/wicket-parsley-validation">this repo</a> you may find an integration with <a href="http://parsleyjs.org/">Parsley.js</a>.</p>
<p>The implementation is in <a href="https://github.com/martin-g/blogs/blob/master/wicket-parsley-validation/src/main/java/com/mycompany/parsley/ParsleyValidationBehavior.java">ParsleyValidationBehavior</a>.<br />
This is the base class that does most generic configuration, i.e. set the expected by Parsley <em>data-xyz</em> attributes in the form component markup. Additionally it uses a delegate <em title="org.apache.wicket.validation.IValidator">IValidator</em> that does the server side validation.</p>
<p>The usage is as any other validator in Wicket:</p>

{% highlight java %}
TextField<String> fullName = new TextField<String>("fullname", someModel);
fullName.add(new ParsleyValidationBehavior<String>(EmailValidator.getInstance()).type("email").require(true));
{% endhighlight %}

<p>The code above is a bit mouthfull so we can create a specialization class:</p>

{% highlight java %}
public class ParsleyEmailValidator extends ParsleyValidationBehavior<String>
{
	public ParsleyEmailValidator()
	{
		super(EmailAddressValidator.getInstance());

		require(true);
		type("email");
	}
}
{% endhighlight %}

<p>and use it like:</p>

{% highlight java %}
TextField<String> fullName = new TextField<String>("fullname", someModel);
fullName.add(new ParsleyEmailValidator());
{% endhighlight %}

<p>The project just demostrates how to approach the problem. It does not provide full integration with Parsley.js.<br />
If there is interest the project can be moved to <a href="https://github.com/wicketstuff/core">WicketStuff</a> and extended more with <strong>your</strong> help!</p>
<p><strong>Update</strong>: <a href="https://twitter.com/Cedric_Gatay">Cedric Gatay</a> extended the demo application with Bean Validation (JSR 303) support: <a href="https://github.com/code-troopers/wicket-jsr303-parsley">Wicket + JSR_303 + Parsley</a>. Good work, Cedric!</p>
