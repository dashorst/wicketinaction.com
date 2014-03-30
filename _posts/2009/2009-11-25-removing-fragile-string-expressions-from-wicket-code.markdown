---
layout: post
status: publish
published: true
title: Removing Fragile String Expressions From Wicket Code
author: ivaynberg
author_login: ivaynberg
author_email: igor.vaynberg@gmail.com
excerpt: "The most used model in Wicket is the <code>PropertyModel</code> and its
  derivatives. This model allows the user to quickly and easily navigate object graphs
  using string-based property-path expressions, and is used extensively when displaying
  data or creating forms.\r\n\r\nHowever, the <code>PropertyModel</code> and its string-based
  expressions are also responsible for adding the most fragility to Wicket code. The
  standard Java tooling does not support refactoring strings that happen to contain
  references to java constructs such as methods or fields; thus errors in these string
  expressions are not discovered until runtime.\r\n\r\nFor example, the string used
  in the code below:\r\n<pre lang=\"java\">new Label(\"state\",\r\n        new PropertyModel(personModel,
  \"address.state.code\"));</pre>\r\nwill not be updated by the tooling when the getState()
  method is renamed into getArea() and will thus cause an error only discoverable
  at runtime.\r\n\r\nSo why is <code>PropertyModel</code> used so often in Wicket
  code even though it causes so many problems? The answer is simple: it is, by far,
  the easiest and most convenient way to achieve the functionality. We chose to sacrifice
  robustness of our code for development ease and convenience. What if there was another
  way to achieve the same, but without using strings?\r\n"
wordpress_id: 470
wordpress_url: http://wicketinaction.com/?p=470
date: '2009-11-25 23:14:16 +0100'
date_gmt: '2009-11-25 21:14:16 +0100'
categories:
- Wicket
tags: []
comments:
- id: 612
  author: Tweets that mention Removing Fragile String Expressions From Wicket Code
    | Wicket in Action -- Topsy.com
  author_email: ''
  author_url: http://topsy.com/tb/bit.ly/4qkqFr
  date: '2009-11-26 00:35:39 +0100'
  date_gmt: '2009-11-25 22:35:39 +0100'
  content: '[...] This post was mentioned on Twitter by ildella and Planet Apache,
    Igor Vaynberg. Igor Vaynberg said: PropertyModels without Strings are here: http://bit.ly/4qkqFr
    [...]'
- id: 613
  author: Francisco
  author_email: franciscotreacy@yahoo.fr
  author_url: ''
  date: '2009-11-26 10:33:49 +0100'
  date_gmt: '2009-11-26 08:33:49 +0100'
  content: "Wow, neat but what a mess!\r\n\r\nFortunately there is Scala for some
    of us.\r\n\r\n@serializable\r\nclass SafePropertyModel[T](expression: =&gt; T)
    extends IModel[T] {\r\n\r\ndef getObject: T = {\r\n    try {\r\n      expression\r\n
    \   } catch {\r\n      case npe: NullPointerException =&gt; null.asInstanceOf[T]\r\n
    \   }\r\n  }\r\n}\r\n\r\ncase class Address(val street: String)\r\ncase class
    Person(val address: Address)\r\n\r\nval p = Person(Address(\"Some street\"))\r\nval
    p_null = Person(null)\r\n\r\nval model = new SafePropertyModel(p.address.street)\r\nval
    model_null = new SafePropertyModel(p_null.address.street)\r\n\r\nval result =
    model.getObject // result is \"Some street\"\r\nval result_null = model_null.getObject
    // result is null\r\n\r\nI haven't compiled this code, but that should be about
    it."
- id: 614
  author: WicketCoder
  author_email: wicketcoder@mailinator.com
  author_url: ''
  date: '2009-11-26 14:05:00 +0100'
  date_gmt: '2009-11-26 12:05:00 +0100'
  content: "Whats wrong with:\r\n\r\nadd(new Label(\"state\", new Model() {\r\n        @Override\r\n
    \       public Object getObject() {\r\n            return person.getAddress().getSate().getCode();\r\n
    \       }\r\n    }));"
- id: 615
  author: dashorst
  author_email: martijn.dashorst@gmail.com
  author_url: http://martijndashorst.com
  date: '2009-11-26 14:12:42 +0100'
  date_gmt: '2009-11-26 12:12:42 +0100'
  content: I see 3 possible NPE's in your code. It isn't reusable. It adds to the
    permgen. It is a lot more typing. If person changes, your code doesn't detect
    the change.
- id: 616
  author: WicketCoder
  author_email: wicketcoder@mailinator.com
  author_url: ''
  date: '2009-11-26 14:20:11 +0100'
  date_gmt: '2009-11-26 12:20:11 +0100'
  content: "Not resuable - how come?\r\nAdds to the permgen? Wow scraping the barrel
    with what one\r\nMore typing? Not really I use an IDE\r\nDont get your last point.\r\n\r\nKISS."
- id: 617
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-26 18:27:14 +0100'
  date_gmt: '2009-11-26 16:27:14 +0100'
  content: "@WicketCoder\r\n\r\nCertainly this is the best thing one can do, but\r\n\r\nthe
    problems with your code are:\r\na) like Martijn mentioned NPEs.\r\nb) you are
    missing the setter\r\nc) you are missing the detach call propagation because 99%
    of the time person is an IModel\r\n\r\nby the time you are done you end up with:\r\nadd(new
    Label(”state”, new Model() {\r\n@Override\r\npublic Object getObject() {\r\nPerson
    _person=person.getObject();\r\nif (_person!=null&&_person.getAddress()!+null&&_person.getAddress.getState()!=null)
    {\r\nreturn _person.getAddress().getSate().getCode();\r\n} else return null;\r\n}\r\n\r\npublic
    void setObject(Object v) {\r\nperson.getObject().getAddress().getSate().setCode(v);\r\n}\r\n\r\npublic
    void detach() {\r\n if (person!=null)\r\n {\r\n   person.detach();\r\n  }\r\n}\r\n\r\n\r\n}));\r\n\r\n\r\nNow
    imagine a form with 10 fields, it wont look pretty. I, certainly, would not want
    to read that code. Like I said in the article, PropertyModel is extremely popular
    because its easy and concise, the alternative is far too verbose."
- id: 618
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-26 18:28:23 +0100'
  date_gmt: '2009-11-26 16:28:23 +0100'
  content: '@Francisco wow, Scala looks nice, but what a mess!'
- id: 619
  author: Dotchev
  author_email: nomadik@mail.bg
  author_url: ''
  date: '2009-11-27 10:16:24 +0100'
  date_gmt: '2009-11-27 08:16:24 +0100'
  content: "PropertyModel uses reflection and reflection is bad for reasons we all
    know.\r\n\r\nI think the major problem here is that everyone uses properties in
    their Java programs but strangely Java language knows nothing of properties -
    a major language deficiency IMHO.\r\n\r\nAnyway, probably with closures things
    will get better (when wicket moves to Java 7)."
- id: 620
  author: Paul
  author_email: paul.mooney@live.com
  author_url: ''
  date: '2009-11-27 17:56:52 +0100'
  date_gmt: '2009-11-27 15:56:52 +0100'
  content: "I agree with Dotchev that the lack of compile time safe, refactorable
    means to reference properties is a huge language deficiency.\r\n\r\nThis bindgen
    generator, much like JPA 2's typesafe criteria API seems a bit weird. It seems
    to me that anything that will be processed and anything generated has to go into
    a separate project/module. Is there a way this would work if the @Bindable and
    generated classes reside in the same project/module?"
- id: 621
  author: WicketCoder
  author_email: wicketcoder@mailinator.com
  author_url: ''
  date: '2009-11-27 19:09:32 +0100'
  date_gmt: '2009-11-27 17:09:32 +0100'
  content: "Not necessarily nulls could be checked for elsewhere.\r\nDo you check
    null check every reference in your application before using it?"
- id: 622
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-27 21:25:45 +0100'
  date_gmt: '2009-11-27 19:25:45 +0100'
  content: '@Paul you dont need a separate project because the generated sources are
    generated into a separate folder and are never checked into the scm - they are
    regenerated on every compilation.'
- id: 623
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-27 21:28:04 +0100'
  date_gmt: '2009-11-27 19:28:04 +0100'
  content: '@WicketCoder in business layer i would expect an NPE, but in the UI layer
    where i am interested in simply displaying the value to the user I would not want
    to make sure that the domain model contains dummy associations down to the primitives
    level.'
- id: 624
  author: Paul
  author_email: paul.mooney@live.com
  author_url: ''
  date: '2009-11-27 23:31:26 +0100'
  date_gmt: '2009-11-27 21:31:26 +0100'
  content: '@ivaynberg Now I understand: bindgen reads source and generates source,
    not compiled classes.'
- id: 625
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-28 00:46:20 +0100'
  date_gmt: '2009-11-27 22:46:20 +0100'
  content: '@paul you got it'
- id: 626
  author: Francisco
  author_email: franciscotreacy@yahoo.fr
  author_url: ''
  date: '2009-11-29 14:02:59 +0100'
  date_gmt: '2009-11-29 12:02:59 +0100'
  content: "@ivaynberg:\r\n\r\nThe Scala solution a mess? I really can't see why.\r\n\r\n
    - No external libraries/processors/build systems required.\r\n - ~20 LOC in *one*
    file including the model itself, the domain classes, and two examples.\r\n - No
    strange vocabulary (\"binding\") in the code.\r\n\r\nI understand if you can't
    or don't want to use Scala. But I don't know how much more signal vs. noise you
    can get. Check out the Wicket with Scala presentation by Jan Kriesten."
- id: 627
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-29 19:58:19 +0100'
  date_gmt: '2009-11-29 17:58:19 +0100'
  content: "@Francisco\r\n\r\nwell you did not bother qualifying why Bindgen solution
    was a mess, so i did not bother qualifying why Scala was either.\r\n\r\n> No external
    libraries/processors/build systems required.\r\n\r\nBindgen uses a feature built
    directly into the javac compiler itself so it is not an \"exteral\" build tool,
    it is just a library that provides a compiler plugin. i was not aware that Scala
    was so complete that no one needs to write any libraries at all to build any kind
    of application. that is great to hear, maybe i will give it a try one day.\r\n\r\n>
    ~20 LOC in *one* file including the model itself, the domain classes, and two
    examples.\r\n\r\nso? i can rewrite it using Brainfuck and make it look like ascii
    art, which i think is way cooler. can you make a domain class look like ascii
    art in Scala? or perhaps i can use Whitespace and make it look like its zero lines
    of code.\r\n\r\nin either case it will be irrelevant to this discussion, seeing
    how this is about Java and not [insert your favorite language that can do this
    way better here]\r\n\r\n> No strange vocabulary (”binding”) in the code.\r\n\r\nbut
    your code has this weird \"SafePropertyModel\" i have to know about, weird..."
- id: 628
  author: Steve
  author_email: slowery@gatessolutions.com
  author_url: ''
  date: '2009-11-30 16:25:35 +0100'
  date_gmt: '2009-11-30 14:25:35 +0100'
  content: Does this Bindgen work at all with CompoundPropertyModel?  Doesn't seem
    like it would, and we primarily use the CPM due to how clean it keeps the java
    code.
- id: 629
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-30 18:04:27 +0100'
  date_gmt: '2009-11-30 16:04:27 +0100'
  content: '@Steve the whole point of using Bindgen is that you remove the dependence
    on fragile strings from your code, in this view the CPM is flawed at the very
    core.'
- id: 630
  author: Francisco
  author_email: franciscotreacy@yahoo.fr
  author_url: ''
  date: '2009-11-30 18:19:19 +0100'
  date_gmt: '2009-11-30 16:19:19 +0100'
  content: "@ivaynberg\r\n\r\nI didn't mean to offend you with the word mess. Sorry.
    It's not worth to make a big deal out of it.\r\n\r\nI'll rephrase it then, *I*
    think that the solution in Scala is neat, because I think the code is more expressive
    and because I don't have to rely on Maven, a tool I dislike.\r\n\r\nI thought
    it would be a good idea to share that alternative, since Wicket can be effectively
    used from Scala today (in contrast to Brainfuck and Whitespace - that IMO have
    a lower s/n ratio).\r\n\r\nIt might enlighten some people, who knows, that surprisingly
    might be trying to \"remove fragile string expressions from Wicket code\". That's
    why I think it is not irrelevant, and it's fine if you disagree."
- id: 631
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-11-30 18:21:36 +0100'
  date_gmt: '2009-11-30 16:21:36 +0100'
  content: '@Francisco i wasnt offended, just annoyed; and not by the word, but by
    the lack of explanation :)'
- id: 633
  author: Peter
  author_email: pertl@gmx.org
  author_url: ''
  date: '2009-12-01 20:20:03 +0100'
  date_gmt: '2009-12-01 18:20:03 +0100'
  content: bindgen is really cool. Did anyone run it successfully in IDEA 9? I still
    have some issues with test classes (src/test/java) not finding the main classes
    (src/main/java) during apt processing. I did enable 'Annotation Processing' in
    IDEA *btw*
- id: 635
  author: Nico
  author_email: nguba@bioware.com
  author_url: http://www.swtor.com
  date: '2009-12-02 04:14:37 +0100'
  date_gmt: '2009-12-02 02:14:37 +0100'
  content: "Maybe what needs addressing is the way the PropertyModel (or similar)
    is binding the values, rather than fixing how the values are named (referenced).
    \ \r\n\r\nAnnotations to the rescue?"
- id: 636
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-12-02 04:48:19 +0100'
  date_gmt: '2009-12-02 02:48:19 +0100'
  content: '@Peter im an Eclipse user myself, but maybe this will help: http://blogs.jetbrains.com/idea/2009/11/userfriendly-annotation-processing-support-jpa-20-metamodel/'
- id: 637
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-12-02 05:52:27 +0100'
  date_gmt: '2009-12-02 03:52:27 +0100'
  content: "@Francisco if i read your code correctly there is a pretty huge gap left
    out that your SafePropertyModel does not take care of...\r\n\r\nyour code says
    p.address.street which, with my non-existant knowledge of all things Scala, creates
    a lambda expression to be avaluated later? but, the expression is tied to a specific
    instance of the root object \"p\". can you show an easy way to achieve the same
    but with interchangeable \"p\"? so in other words:\r\n\r\n<pre language=\"java\">\r\nnew
    BindingModel(personModel, new PersonBinding().address.street())\r\n</pre>\r\n\r\nthe
    above creates a \"address.street\" expression that is evaluated against which
    ever \"p\" is provided by the personModel..."
- id: 638
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-12-02 06:49:45 +0100'
  date_gmt: '2009-12-02 04:49:45 +0100'
  content: '@Nico mind elaborating? im not sure i get what you mean. the problem is
    that java has no way to express a chain of method calls easily, this is why we
    resort to things like strings...'
- id: 644
  author: Francisco
  author_email: franciscotreacy@yahoo.fr
  author_url: ''
  date: '2009-12-07 16:19:53 +0100'
  date_gmt: '2009-12-07 14:19:53 +0100'
  content: "Hi Igor,\r\n\r\nSure. I haven't really thought about that, but I guess
    it could be solved with an implicit conversion?\r\n\r\nSo instead of:\r\n\r\nval
    model = new SafePropertyModel(p.address.street)\r\n\r\nby introducing the following
    (or mixing-in a trait with it):\r\n\r\nimplicit def m2o[A](model: IModel[A]):
    A = model.getObject\r\n\r\nwe can now invoke the expression on a model:\r\n\r\n//
    this is a concrete instance of a person model\r\nobject personModel extends IModel[Person]
    {\r\n  def getObject = Person(Address(\"My model's street\"))\r\n}\r\n\r\nval
    model = new SafePropertyModel(personModel.address.street)\r\n\r\nNow,\r\n\r\nassert(model.getObject
    == \"My model's street\")\r\nprintln(model.getObject)\r\n\r\nAll it does is convert
    under the hood a model to an object when needed. Is this what you meant?\r\n\r\nFrancisco"
- id: 645
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-12-07 18:48:09 +0100'
  date_gmt: '2009-12-07 16:48:09 +0100'
  content: "@Francisco not really sure, maybe its easier with code :)\r\n<pre>\r\nPerson
    bob=new Person(\"bob\");\r\nPerson john=new Person(\"bob\");\r\n\r\nIModel model=new
    BindingModel(person, new \r\nPersonBinding().address().street());\r\n\r\nmodel.getObject()
    &lt;== bob.getAddress().getStreet();\r\nmodel.setObject(john);\r\nmodel.getObject()
    &lt;== john.getAddress().getStreet();\r\n</pre>"
- id: 646
  author: Francisco
  author_email: franciscotreacy@yahoo.fr
  author_url: ''
  date: '2009-12-07 20:03:02 +0100'
  date_gmt: '2009-12-07 18:03:02 +0100'
  content: "Alright, I see what you mean.\r\n\r\nIn that case I would just do:\r\n\r\nobject
    personModel extends IModel[Person] {         \r\n   var p = Person(Address(\"first\"))\r\n
    \  def getObject = p\r\n   def setObject(p: Person) = { this.p = p }\r\n}\r\n\r\n\r\nmodel.getObject\r\n//
    here i would obtain first\r\n\r\npersonModel.setObject(Person(Address(\"second\")))\r\n\r\nmodel.getObject\r\n//
    here i would obtain second\r\n\r\nBut most probably I can't see in which cases
    I would benefit from that (a \"generic safe street model\" :). And afaik that
    is not possible with PropertyModel, or is it?\r\n\r\nAnyways, in general I tend
    to avoid setters whenever possible... they're evil =)"
- id: 647
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-12-07 20:17:06 +0100'
  date_gmt: '2009-12-07 18:17:06 +0100'
  content: "@Francisco\r\n\r\nIt is possible with a PropertyModel. BindingModel is
    exactly the same as the PropertyModel, the only exception being that it uses a
    type-safe expression instead of a string to specify the property."
- id: 660
  author: Paul
  author_email: paul.mooney@live.com
  author_url: ''
  date: '2010-01-14 17:55:22 +0100'
  date_gmt: '2010-01-14 15:55:22 +0100'
  content: "If you're using hibernate 3.5, then I don't think you need bindgen to
    accomplish this, at least when dealing with entities which is probably most of
    the time. It has it's static typesafe metamodel generator. Tutorial to set it
    up can be found here: http://relation.to/Bloggers/HibernateStaticMetamodelGeneratorAnnotationProcessor\r\n\r\nI've
    just been glancing at it so I don't know if this can be a complete solution. If
    it can just be used directly with the PropertyModel, or even if you can get complex
    property strings out of it (or even if you'd want to)."
- id: 661
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2010-01-15 06:01:26 +0100'
  date_gmt: '2010-01-15 04:01:26 +0100'
  content: "@Paul, i would guess that while Entities take up a large usecase space
    for this kind of object navigation they still only account for about 60%. In Wicket
    you still bind a lot to non-entities such as DTOs or to Component properties.\r\n\r\nThe
    JPA2 solution doesnt chain properties :( so you cant say new PersonBinding().address().state().code(),
    instead you woul dhave to do something like:\r\nnew EntityBinding(Person_.address,
    Address_.state, State_.code) which is more error prone. And as you said, it only
    works for Entities."
- id: 673
  author: Bert Bruynooghe
  author_email: bert.bruynooghe@gmail.com
  author_url: ''
  date: '2010-05-13 22:45:25 +0200'
  date_gmt: '2010-05-13 20:45:25 +0200'
  content: "I tried something similar using an own class that can be used as follows:\r\nnew
    AccessorModel(modelOrObjectItself){{\r\n  registrar.getFoo();\r\n  registrar.setFoo(null);\r\n}}
    where registrar is of the type InputType, thus allowing auto-complete features
    of the IDE.  I was also thinking of allowing chaining expressions, but as I wasn't
    able to use final classes as InputType, I gave up. However, today I discovered
    PowerMock, and they seem to have the solution to my problem. Is this a path worth
    investigating, and if yes, is anybody interested in trying to work this out in
    an small open source thingie?"
- id: 676
  author: Hielke Hoeve
  author_email: hielke.hoeve@gmail.com
  author_url: ''
  date: '2010-05-28 09:17:00 +0200'
  date_gmt: '2010-05-28 07:17:00 +0200'
  content: "Try using Bindgen in Eclipse in a project with over 1k entity classes
    and try to refactor something (or even better change generics). Coffee and lunch
    anyone? :)\r\n\r\nHalf the time Bindgen does not understand generics and just
    leaves the building..."
- id: 747
  author: Sophie Ortega
  author_email: sophieortega@gmail.com
  author_url: http://sophieortega.co.cc/
  date: '2010-12-24 01:30:10 +0100'
  date_gmt: '2010-12-23 23:30:10 +0100'
  content: '@Francisco not really sure, maybe its easier with code :) Person bob=new
    Person("bob"); Person john=new Person("bob"); IModel model=new BindingModel(person,
    new PersonBinding().address().street()); model.getObject() &lt;== bob.getAddress().getStreet();
    model.setObject(john); model.getObject() &lt;== john.getAddress().getStreet();'
---
<p>The most used model in Wicket is the <code>PropertyModel</code> and its derivatives. This model allows the user to quickly and easily navigate object graphs using string-based property-path expressions, and is used extensively when displaying data or creating forms.</p>
<p>However, the <code>PropertyModel</code> and its string-based expressions are also responsible for adding the most fragility to Wicket code. The standard Java tooling does not support refactoring strings that happen to contain references to java constructs such as methods or fields; thus errors in these string expressions are not discovered until runtime.</p>
<p>For example, the string used in the code below:</p>

{% highlight java %}new Label("state",
        new PropertyModel(personModel, "address.state.code"));
{% endhighlight %}

<p>will not be updated by the tooling when the getState() method is renamed into getArea() and will thus cause an error only discoverable at runtime.</p>
<p>So why is <code>PropertyModel</code> used so often in Wicket code even though it causes so many problems? The answer is simple: it is, by far, the easiest and most convenient way to achieve the functionality. We chose to sacrifice robustness of our code for development ease and convenience. What if there was another way to achieve the same, but without using strings?<br />
<a id="more"></a><a id="more-470"></a><br />
Enter <a href="http://bindgen.org">Bindgen</a>, a Java 6 apt processor <a href="http://help.eclipse.org/galileo/index.jsp?topic=/org.eclipse.jdt.doc.isv/guide/jdt_apt_getting_started.htm">[1]</a><a href="http://java.sun.com/javase/6/docs/api/javax/annotation/processing/Processor.html">[2]</a> capable of generating code that can represent the same property-expressions but using real Java objects instead of strings.</p>
<p>Bindgen allows the developer to transform code like this:</p>

{% highlight java %}
new Label("state",
        new PropertyModel(personModel, "address.state.code"));
{% endhighlight %}

<p>into code like this:</p>

{% highlight java %}
new Label("state",
        BindingModel.of(personModel,
                new PersonBinding().address().state().code()));
{% endhighlight %}

<p>No more strings! And although standard Java tooling will not automatically rename <code>state()</code> when the field or method it references is renamed, the compiler will immediately report all places that need to be updated via a compilation error.</p>
<p>So what does Bindgen get you? It gets you the same ease and convenience of <code>PropertyModels</code> but with the added compile-time safety. Too good to be true? Try it out for yourself…</p>
<h3>Setting up the environment</h3>
<p>The following demonstrates how to set up a project that is built using maven and eclipse. This code is experimental, and there is not much documentation yet (see Sources section below), but what Bindgen does is worth trying.</p>
<p>Adjust your pom as described here: <a href="http://code.google.com/p/bindgen-wicket/wiki/ConfiguringMaven2">http://code.google.com/p/bindgen-wicket/wiki/ConfiguringMaven2</a></p>
<p>Now run <code>mvn eclipse:eclipse</code> and the project should be ready to go using both eclipse and regular maven builds.</p>
<h3>Using Bindgen</h3>
<p>Using Bindgen itself is simple. Annotate a class whose properties you wish to access using the <code>@org.bindgen.Bindable</code> annotation and Bindgen will generate a <code>&lt;ClassName&gt;Binding</code> class that can be used to create <code>Binding</code> instances that represent property expressions. Eclipse will generate these classes as soon as the class with <code>@Bindable</code> is saved, so the <code>&lt;ClassName&gt;Binding</code> class should be available as soon as you press <kbd>Ctrl+S</kbd> after adding the <code>@Bindable</code> annotation.</p>
<p>For example:</p>
<p>Given</p>

{% highlight java %}
public class Address { public String city; }

@Bindable public class Person { public Address address; }
{% endhighlight %}

<p>Bindgen will generate classes <code>AddressBinding</code> and <code>PersonBinding</code> enabling code like this:</p>

{% highlight java %}
Binding binding=new PersonBinding().address().city();
String city=binding.get();
binding.set("Beverly Hills");
assertEquals("address.city", binding.getPath());
{% endhighlight %}

<h3>Configuring Bindgen</h3>
<p>Bindgen will generate bindings for all classes reachable from the annotated class. Most of the time this is not desirable, eg we do not necessarily want binding classes generated for <code>java.lang.String</code> or for <code>com.caucho.hessian.io.SerializerFactory</code> just because they are reachable from a class with the <code>@Bindable</code> annotation. Bindgen provides a way to limit the generation of binding classes to certain packages by specifying a comma separated list of these packages.  To do this, create a file called <code>bindgen.properties</code> in the project’s base folder (the one that contains the pom file) and add the scope property, eg</p>

{% highlight properties%}
scope=com.myproject.mypackage,com.myproject.myotherpackage
{% endhighlight %}

<h3>Using Bindgen with Wicket</h3>
<p>The bindgen-wicket module provides classes that facilitate integration between Wicket and Bindgen. These are <code>BindingRootModel</code> and <code>BindingColumn</code>.</p>
<p><code>BindingRootModel</code> is the replacement for the <code>PropertyModel</code>, and its usage looks like this:</p>

{% highlight java %}
add(new Label("state",
        new BindingRootModel(personModel,
                new PersonBinding().address().state().code()));
{% endhighlight %}
<p>Or using a convenience wrapper:</p>

{% highlight java %}
add(new Label("state",
        BindingModel.of(personModel,
                new PersonBinding().address().state().code()));
{% endhighlight %}

<p>The <code>BindingColumn</code> class allows binding expressions to be used for creating <code>DataTable</code> columns. Eg:</p>

{% highlight java %}
new BindingColumn(
                 new PersonBinding().address().state().code())
         .setHeader("column.state")
{% endhighlight %}

<h3>Conclusion</h3>
<p>As we can see, Bindgen is able to remove one of the biggest pain points in Wicket code, and do so in a manner mostly transparent to the developer. Wicket is not the only framework that can benefit from Bindgen, any place in code where property expressions are kept in strings can be upgraded to a compile-safe Bindgen binding and if that is not an option at least the property expression string can itself be generated from a binding using the <code>Binding#getPath()</code> method.</p>
<p>It is important to keep in mind that a lot of the code that makes all this possible is still fresh, and so there may be a bug here or there. Your feedback would be very appreciated.</p>
<h3>Sources</h3>
<p>Bindgen website: <a href="http://www.bindgen.org">http://www.bindgen.org</a></p>
<p>My fork of bindgen repository which contains my latest tweaks: <a href="http://github.com/ivaynberg/bindgen">http://github.com/ivaynberg/bindgen</a></p>
<p>Bindgen official repository, this is where my changes eventually get merged: <a href="http://github.com/stephenh/bindgen">http://github.com/stephenh/bindgen</a></p>
<p>Bindgen-Wicket integration project: <a href="http://code.google.com/p/bindgen-wicket/">http://code.google.com/p/bindgen-wicket</a></p>
<p>Demo project: <a href="http://bindgen-wicket.googlecode.com/svn/trunk/bindgen-wicket-examples/">http://bindgen-wicket.googlecode.com/svn/trunk/bindgen-wicket-examples/</a></p>
