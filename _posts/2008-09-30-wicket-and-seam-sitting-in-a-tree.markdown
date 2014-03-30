---
layout: post
status: publish
published: true
title: Wicket and Seam sitting in a tree
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 186
wordpress_url: http://wicketinaction.com/2008/09/wicket-and-seam-sitting-in-a-tree/
date: '2008-09-30 23:41:07 +0200'
date_gmt: '2008-09-30 21:41:07 +0200'
categories:
- Wicket
tags: []
comments:
- id: 492
  author: Cruz
  author_email: cruz.fernandez@teracode.com
  author_url: http://www.teracode.com
  date: '2009-05-08 17:24:05 +0200'
  date_gmt: '2009-05-08 15:24:05 +0200'
  content: Doesn't have this page a serialization problem? If it is serialized won't
    the SEAM objects be serialized copies from the SEAM contextual objects?
---
<p><a href="http://in.relation.to/Bloggers/Pete" title="In Relation To...  Pete Muir">Peter Muir</a> of the Seam team has written up an introduction into using <a href="http://wicket.apache.org/" title="Apache Wicket - Home">Wicket</a> and <a href="http://www.jboss.com/products/seam" title="JBoss.com - JBoss Seam">Seam</a> together.</p>
<div style="display: none"><a href='http://essaywritingservicee.com/' title='custom essay writing services'>custom essay writing services</a></div></p>
<p>
He starts by stripping JSF support from the default Seam setup (and it never gets uncomfortable writing this down), and adding Wicket specifics to it. Peter starts with a simple Hello, World! page, which has nothing specific to Seam. But then things get interesting with implementing an authentication form using Seam&#039;s bijections.</p>
<p>
	The code is quite easy to follow (I&#039;ve merged the final resulting form class, and applied a <code>CompoundPropertyModel</code> to save on some code):</p>
{% highlight java %}public class LoginForm extends Form {
    @In Identity identity;
    @In Credentials credentials;
		
    public LoginForm(String id) {
        super(id, new CompoundPropertyModel(credentials));
        add(new TextField("username"));
        add(new PasswordTextField("password"));
    }
		
    protected void onSubmit() {
        try {
            identity.authenticate();
            setResponsePage(HomePage.class);
        } catch (LoginException e) {
            error("Login failed");                    
        }
    }   
}{% endhighlight %}
<p>This looks very readable. Read more about Wicket and Seam on the <a href="http://in.relation.to/Bloggers/SeamlessWicket">Seam Blog</a>.</p>
<div style="display: none">zp8497586rq</div>
