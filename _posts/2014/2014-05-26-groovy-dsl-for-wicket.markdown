---
layout: post
status: publish
published: true
title: Groovy DSL for Apache Wicket
author: dashorst
date: '2014-05-26 13:08:52 +0100'
date_gmt: '2014-05-26 11:08:52 +0100'
categories:
- Wicket
tags: [groovy]
comments: []
---
Eugene Kamenev has released his take on [making Wicket groovy][1]:

> Apache Wicket is great, and Groovy is also great. This project tries
> to combine the power of both. However, sometimes Apache Wicket code
> become damn verbose. But with some little, yet powerful Groovy DSL
> written, we can extend Wicket to simplify common tasks, and to delete
> over 30-40% of verbose code.

A short example of the usual Wicket and Java code:

{% highlight java %}
...
Form form = new Form("wicketForm", new CompoundPropertyModel(this)) {
    @Override
    public void onSubmit(){
        System.out.println(MyPage.this.input1 + MyPage.this.input2);
    }
    @Override
    public boolean isVisible() {
        // just for example :)
        !MyPage.this.input1.equals(MyPage.this.input2);
    }
}
form.add(new TextField("input1"));
form.add(new TextField("input2"));
add(form);
{% endhighlight %}

These lines of code can be shortened by using Eugene's Groovy DSL:

{% highlight groovy %}
...
use(WicketDSL, WicketFormDSL) {
    def form('wicketForm', 
            new CompoundPropertyModel(this), 
            [
             submit:  { println this.input1 + this.input2 },
             visible: { this.input1 != this.input2 } 
            ]
        )
    form.field('input1')
    form.field('input2')
    this << form
}
{% endhighlight %}

You can see some example code in his readme file, and the project comes
with some examples.

[Go check it out][1]!

[1]: https://github.com/eugene-kamenev/wicket-groovy-DSL