---
layout: post
status: publish
published: true
title: Building a smart EntityModel
author: ivaynberg
author_login: ivaynberg
author_email: igor.vaynberg@gmail.com
excerpt: "A lot of Wicket applications use an ORM framework to work with the database.
  Because ORMs provide a generic mechanism for loading entities we can create a generic
  Wicket model that can simplify binding between ORM and Wicket. In this article I
  will walk you through creating an EntityModel that will make using Wicket and ORM
  fun, read inside...\r\n\r\n"
wordpress_id: 84
wordpress_url: http://wicketinaction.com/?p=84
date: '2008-09-30 05:33:30 +0200'
date_gmt: '2008-09-30 03:33:30 +0200'
categories:
- Wicket
tags:
- Wicket
- orm
- Database
- entity
- transactions
comments:
- id: 2
  author: First Wicket article by Igor Vaynberg | A Wicket Diary
  author_email: ''
  author_url: http://martijndashorst.com/blog/2008/09/30/first-wicket-article-by-igor-vaynberg/
  date: '2008-09-30 08:57:46 +0200'
  date_gmt: '2008-09-30 06:57:46 +0200'
  content: '[...] It is a great day: Igor Vaynberg, the most prominent committer for
    Apache Wicket (he has the most commits I think) has written up an article about
    building a smart EntityModel. You can read it at the official Wicket blog: wicketinaction.com
    [...]'
- id: 3
  author: Bernard
  author_email: bernard@herge21.net
  author_url: ''
  date: '2008-09-30 10:46:08 +0200'
  date_gmt: '2008-09-30 08:46:08 +0200'
  content: "Hi,\r\nGreat article...\r\n\r\nSmall improvement... It is possible to
    deduct the class of T by using this expression: (Class) ((ParameterizedType) getClass()
    \           .getGenericSuperclass()).getActualTypeArguments()[0]. By using this
    trick, you don't need to include the clazz parameters in the constructor and in
    the load method.\r\n\r\nCheers,\r\nBernard."
- id: 4
  author: Marcell
  author_email: barbacena@gmail.com
  author_url: ''
  date: '2008-09-30 11:06:05 +0200'
  date_gmt: '2008-09-30 09:06:05 +0200'
  content: Nice article. Shouldn't Identifiable extends Serializable?
- id: 5
  author: Chris
  author_email: chris_overseas@hotmail.com
  author_url: ''
  date: '2008-09-30 12:08:43 +0200'
  date_gmt: '2008-09-30 10:08:43 +0200'
  content: Thanks Igor, some useful tricks here, especially for someone like myself
    new to both Wicket &amp; Hibernate. I'd love to see a followup article to see
    how you handle more complex situations, eg collections of entities filtered by
    certain criteria.
- id: 6
  author: Chris
  author_email: chris_overseas@hotmail.com
  author_url: ''
  date: '2008-09-30 12:22:35 +0200'
  date_gmt: '2008-09-30 10:22:35 +0200'
  content: 'Bernard: that works in the simple case however if you have a deeper class
    hierarchy where the type argument is declared elsewhere (on either a superclass
    or an interface) the code gets a more complex. You have to recurse up via getSuperclass()
    and check both the class and any interfaces for the appropriate param. With multiple
    type params it''s even messier, you have to recurse to the top of the hierarchy
    then search downwards, filling in the type params as you find them (otherwise
    you don''t know which actual type param corresponds to which parameter).'
- id: 7
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-09-30 16:20:12 +0200'
  date_gmt: '2008-09-30 14:20:12 +0200'
  content: "@Marcell: There are two things here: 1) Identifiable should not extend
    Serializable because serialization of the object is not Identifiable's concern.
    It simply represents something with a serializable id. If you expect all your
    identifiables to also be serializable you should probably create a new interface
    that extends both. 2) Serializable is only required if you give the model a transient
    entity, so only those entities need to be serializable.\r\n\r\nMy original code
    had the serialization check built into detach(), but the article was getting a
    little too long - thats why I hinted at a second part if there was interest."
- id: 8
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-09-30 16:24:20 +0200'
  date_gmt: '2008-09-30 14:24:20 +0200'
  content: '@Chris: I dont think a follow up to this particular article would include
    querying, but you can definitely take a look at wicket-phonebook project in wicket-stuff
    svn. It demonstrates filtering resultsets.'
- id: 9
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-09-30 16:26:22 +0200'
  date_gmt: '2008-09-30 14:26:22 +0200'
  content: '@Bernard: I agree with Chris, deducing would be nice but hairy. Are there
    any utility libraries out there that have the entire tracing built into a convenient
    function?'
- id: 10
  author: Iman Rahmatizadeh
  author_email: iman.rahmatizadeh@gmail.com
  author_url: ''
  date: '2008-10-01 13:12:08 +0200'
  date_gmt: '2008-10-01 11:12:08 +0200'
  content: Igor I have a similar model already in my app. But, there's a gotcha regarding
    to using it with the DiskStore. Imagine I have a non-persistent entity, where
    the getId() would return null. Now the model would keep the entity in memory instead
    of the id. I start editing the model, pass it to another page, in that page i
    edit it, and then return to the previous page instance. This instance would fetch
    the model from the DiskStore, which would have the previous version of my entity.
    So all my changes when I get back is lost. Do you think we can provide a solution
    for this ?
- id: 11
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-02 02:29:01 +0200'
  date_gmt: '2008-10-02 00:29:01 +0200'
  content: '@Iman: Unfortunately passing objects between pages is a known limitation
    with the diskstore. Currently the stored data would look like this [page1 [model]]
    [page2 [model'']], in order to make this work we would have to rewrite the store
    to be smarter and store the objects like so [page1] [page2] [model], but this
    is a pretty big rewrite. You should open a JIRA issue and provide a quickstart,
    I dont think we will get to it within 1.3 or even 1.4 timeframe, but we will definitely
    try. In the meanwhile I tend to use Panels instead of Pages for situations like
    these; its not ideal but at least its a feasible workaround.'
- id: 13
  author: Marcell
  author_email: barbacena@gmail.com
  author_url: ''
  date: '2008-10-03 13:41:06 +0200'
  date_gmt: '2008-10-03 11:41:06 +0200'
  content: "@Igor: Then at detach you check if the entity has no id and is Serializable.
    What do you do if the entity does not implement Serializable? I mean besides raising
    a error :P\r\n\r\n@Iman: I am using Seam with Wicket and I can say that your problem
    is a good use case to use conversational context provided by Seam."
- id: 14
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-03 18:12:48 +0200'
  date_gmt: '2008-10-03 16:12:48 +0200'
  content: "@Marcell: There isn't much you can do if it is not Serializable, that
    is the contract of the EntityModel if you choose to put in a transient entity.
    In my actual model I have checks that enforce the contract by trying to serialize
    the entity in dev mode. Perhaps I will revisit this article and add those checks.\r\n\r\nI
    also do not think seam's convesational context would be much help to Iman. The
    problem is that the IModel that goes from page A to page B is cloned and now page
    A is no longer aware of the changes to the model that page B made. Its a much
    more deeply rooted problem then just ensuring that the Session instance is the
    same."
- id: 16
  author: Iman Rahmatizadeh
  author_email: iman.rahmatizadeh@gmail.com
  author_url: ''
  date: '2008-10-04 06:22:56 +0200'
  date_gmt: '2008-10-04 04:22:56 +0200'
  content: '@Igor: I wasn''t aware of any workaround with panels. As I use panels
    extensively in my app, would you care to elaborate how it would help in this scenario
    ?'
- id: 17
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-04 06:31:15 +0200'
  date_gmt: '2008-10-04 04:31:15 +0200'
  content: "@Iman: its not really a workaround, more of a way you build your app.
    The problem here is that the model is shared between multiple pages, so the solution
    is to eliminate multiple pages. Instead of navigation consisting of going from
    page to page you can instead keep the user on the same page and accomplish the
    same navigation by swapping out the \"content\" panel.\r\n\r\neg\r\n\r\n<pre>\r\nclass
    ViewUserPanel extends Panel {\r\n  public ViewUserPanel(String id, IModel user)
    {\r\n      add(new Link(\"edit\") {\r\n         onclick() {\r\n             onEditUser(getModel());\r\n
    \        }\r\n      });\r\n   }\r\n\r\n   private void onEditUser() {\r\n        replaceWith(new
    EditUserPanel(getId(), getModel()));\r\n   }\r\n}\r\n</pre>\r\n\r\nSo, instead
    of calling setResponsePage(new EditUserPage()) you swap the panel..."
- id: 18
  author: Marcell
  author_email: barbacena@gmail.com
  author_url: ''
  date: '2008-10-04 11:36:38 +0200'
  date_gmt: '2008-10-04 09:36:38 +0200'
  content: "@Igor: When I mean to use the conversational context you wouldn't store
    the transient Model in the EntityModel. You probably would do at detach and attach:\r\n\r\nif
    (entity != null) {\r\n  if (if (entity.getId() != null) {\r\n    ...\r\n  } else
    {\r\n    Context.getConversationContext().set(this.getClass().getName()+\"#entity\",entity);\r\n
    \ }\r\n}\r\n\r\nAnd at getObject():\r\n\r\nif (entity == null) {\r\n  if (id !=
    null) {\r\n    ...\r\n  } else {\r\n    entity = (T) Context.getConversationContext().get(this.getClass().getName()+\"#entity\");\r\n
    \ }\r\n}"
- id: 19
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-04 18:27:24 +0200'
  date_gmt: '2008-10-04 16:27:24 +0200'
  content: '@Marcell: Sure. I can do the same thing with the Session and a uuid token
    stored in the model. The big advantage of just using the model is that it is its
    own conversational scope. Eg, the user does whatever they need and the conversation
    ends, then the user presses back button 4 times and tries to do something - error
    because conversation has already been closed. This wouldnt happen with the model
    because it would be stored with the page and retrieved again.'
- id: 20
  author: Jörn Zaefferer
  author_email: joern.zaefferer@gmail.com
  author_url: http://bassistance.de
  date: '2008-10-05 23:38:15 +0200'
  date_gmt: '2008-10-05 21:38:15 +0200'
  content: Whats the adavantage of overriding process and doing a rollback when super.process
    returns false, instead of just overriding submit? Submit gets called only when
    the submit is valid, so no transaction gets started unless the submit is actually
    valid.
- id: 26
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-06 06:19:11 +0200'
  date_gmt: '2008-10-06 04:19:11 +0200'
  content: '@Jörn: We want the entity loaded, updated, and saved all in the same transaction.
    onSubmit() is called after wicket has already applied user inputs to the model
    object, at that point you would have to session.merge() your changes instead of
    having them transparently persisted.'
- id: 27
  author: Jörn Zaefferer
  author_email: joern.zaefferer@gmail.com
  author_url: http://bassistance.de
  date: '2008-10-06 15:33:29 +0200'
  date_gmt: '2008-10-06 13:33:29 +0200'
  content: Okay, that makes sense. Thanks for clarifying.
- id: 108
  author: Persistenz für den Feedreader - rattlab.net
  author_email: ''
  author_url: http://www.rattlab.net/2008/10/persistenz-fur-den-feedreader/
  date: '2008-10-28 22:01:43 +0100'
  date_gmt: '2008-10-28 20:01:43 +0100'
  content: '[...] Das NewsModel ist nicht von LoadableDetachableModel abgeleitet,
    da wir keinen schreibenden Zugriff auf das zu speichernde Objekt hätten, sondern
    dieses immer über load() geladen werden müsste. Dennoch ist die Model-Klasse recht
    einfach und übersichtlich. Es speichert die ID und die News selbst, beim Speichern
    in der Session wird die News aber nicht serialisiert. Werden die Daten wieder
    aus der Session geholt (z.B. durch ein Neu Laden der Seite), so wird die News
    anhand der ID erneut aus der Datenbank geladen. Eine allgemein gehaltenere Version
    eines solchen Models beschreibt Igor Vaynberg, einer der Wicket-Mitentwickler,
    im Blogeintrag &#8220;Building a smart EntityModel&#8221;. [...]'
- id: 293
  author: Paul Mogren
  author_email: pmogren@commercehub.com
  author_url: ''
  date: '2008-12-15 18:42:29 +0100'
  date_gmt: '2008-12-15 16:42:29 +0100'
  content: "@Igor \r\nIn reference to comment #18, \r\nI'm trying to find a way to
    achieve this in Wicket 1.3, and having a hard time. It appears that onSubmit()
    is not called within form.process(), and a lot of the methods I might try to override
    for this are declared final. Can you (or anyone) suggest where I can begin and
    end the transaction? THanks"
- id: 294
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-12-15 20:49:09 +0100'
  date_gmt: '2008-12-15 18:49:09 +0100'
  content: "@Paul: I had to do a lot of refactoring in 1.4 to get this working correctly.
    Unfortunately I cannot port this back to 1.3 because it has production apps running
    on it that might depend on the functionality this changed.\r\n\r\nIt is pretty
    easy to set up a single-transaction-per-request pattern in wicket. It is not as
    granular as a transaction for the form submit, but it should still work.\r\n\r\n<pre>
    \r\npublic class WicketRequestCycle extends WebRequestCycle\r\n{\r\n
    \   private TransactionStatus txn;\r\n\r\n    @Dependency\r\n    private PlatformTransactionManager
    txm;\r\n\r\n    public WicketRequestCycle(WebApplication application, WebRequest
    request, Response response)\r\n    {\r\n        super(application, request, response);\r\n
    \   }\r\n\r\n    @Override\r\n    protected void onBeginRequest()\r\n    {\r\n
    \       txn = txm.getTransaction(new DefaultTransactionDefinition());\r\n        super.onBeginRequest();\r\n
    \   }\r\n\r\n    @Override\r\n    protected void onEndRequest()\r\n    {\r\n        super.onEndRequest();\r\n
    \       if (txn != null)\r\n        {\r\n            txm.commit(txn);\r\n            txn
    = null;\r\n        }\r\n    }\r\n\r\n    @Override\r\n    public Page onRuntimeException(Page
    page, RuntimeException e)\r\n    {\r\n        if (txn != null)\r\n        {\r\n
    \           txm.rollback(txn);\r\n            txn = null;\r\n        }\r\n        return
    super.onRuntimeException(page, e);\r\n    }\r\n}\r\n</pre>"
- id: 376
  author: 'Seam / JSF vs Wicket: performance comparison &laquo; Incremental Operations'
  author_email: ''
  author_url: http://ptrthomas.wordpress.com/2009/01/14/seam-jsf-vs-wicket-performance-comparison/
  date: '2009-01-14 17:41:43 +0100'
  date_gmt: '2009-01-14 15:41:43 +0100'
  content: '[...] Implemented what I hope is the exact equivalent of the above using
    only Wicket and JPA, also Maven-ized and Jetty-fied. Decided to also experiment
    with some of the ideas in this blog post. [...]'
- id: 494
  author: Sams
  author_email: sam@stainsby.id.au
  author_url: ''
  date: '2009-05-14 00:24:51 +0200'
  date_gmt: '2009-05-13 22:24:51 +0200'
  content: I've tried the approach of commiting the transaction in onEndRequest in
    the request cycle. The question is what happens if the commit fails - because
    the response has already been sent at that stage. Depending on your database and
    configuration, commits can fail due to IO failures, uniqueness constraint failures,
    collisions, etc. I would still like to do one commit per request since this seems
    ideal for DB4O, but am not sure where to put the commit.
- id: 495
  author: Sams
  author_email: sam@stainsby.id.au
  author_url: http://sam.stainsby.id.au/blog
  date: '2009-05-15 14:10:36 +0200'
  date_gmt: '2009-05-15 12:10:36 +0200'
  content: 'Update: I think I''ve solved this by using a custom RequestCycleProcessor
    and doing the commit in the respond method. Working well so far.'
- id: 501
  author: ReinoutS
  author_email: reinout@gmail.com
  author_url: http://vanschouwen.info/nerdynotes/
  date: '2009-06-04 17:08:28 +0200'
  date_gmt: '2009-06-04 15:08:28 +0200'
  content: "With the example EntityModel code, I'm getting a compile error because
    load() doesn't return a T but instead, an Identifiable.\r\n\r\nAm I missing something?"
- id: 552
  author: Iain Reddick
  author_email: iain.reddick@gmail.com
  author_url: ''
  date: '2009-07-24 13:40:28 +0200'
  date_gmt: '2009-07-24 11:40:28 +0200'
  content: "There is another consideration with AbstractEntityModel.\r\n\r\nIf you
    are working with hibernate-persisted entities, or objects that may be proxied
    in some other way, the object passed in to the constructor may be a proxy (i.e.
    it is of a proxy class, not the OR-mapped class). This obviously causes a problem
    when you come to regenerate the object (in hibernate this will give an unmapped
    entity exception).\r\n\r\nThe solution is to check if the class is a proxy class
    before attempting to load the entity. If it is a proxy, then retrieve the mapped
    class and use that to load.\r\n\r\ne.g. for CGLIB proxies:\r\n\r\nClass entityClass
    = clazz;\r\nif ( net.sf.cglib.proxy.Enhancer.isEnhanced(entityClass) ) {\r\n    //
    is a proxy\r\n    entityClass = clazz.getSuperclass();\r\n}"
- id: 563
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-07-30 19:02:55 +0200'
  date_gmt: '2009-07-30 17:02:55 +0200'
  content: |-
    @Ian: Absolutely. Here is a bit cleaner code if you are working with hibernate:

    <code>public static <T> T unproxy(T entity)
        {
            if (entity instanceof HibernateProxy)
            {
                entity = (T)((HibernateProxy)entity).getHibernateLazyInitializer().getImplementation();
            }
            return entity;
        }</code>
- id: 1169
  author: Adding JPA/Hibernate into the CDI and Wicket Mix | 42 Lines
  author_email: ''
  author_url: https://www.42lines.net/2011/11/21/adding-jpahibernate-into-the-cdi-and-wicket-mix/
  date: '2013-04-26 23:03:34 +0200'
  date_gmt: '2013-04-26 21:03:34 +0200'
  content: '[...] ids, in Wicket we use models. I have written how to create a good
    model to hold entities here: Building a smart EntityModel and expanded on it in
    my book: Apache Wicket Cookbook, so I will not spend a lot of time on it [...]'
---
<p>A lot of Wicket applications use an ORM framework to work with the database. Because ORMs provide a generic mechanism for loading entities we can create a generic Wicket model that can simplify binding between ORM and Wicket. In this article I will walk you through creating an EntityModel that will make using Wicket and ORM fun, read inside...</p>
<p><a id="more"></a><a id="more-84"></a><br />
In order to load an entity from the database we need two pieces of information: entity’s class and id. Since there is no standard way to get an id from an object we will create an interface that will standardize the process:</p>

{% highlight java linenos %}
public interface Identifiable<T extends Serializable>
{
 T getId();
}
{% endhighlight %}

<p>The simplest entity model can then look like this:</p>

{% highlight java linenos %}
public abstract class AbstractEntityModel<T extends Identifiable< ? >>
 extends
 LoadableDetachableModel<T>
{
 private Class clazz;
 private Serializable id;

 public AbstractEntityModel(T entity)
 {
 clazz = entity.getClass();
 id = entity.getId();
 }

 public AbstractEntityModel(Class< ? extends T> clazz, Serializable id)
 {
 this.clazz = clazz;
 this.id = id;
 }

 @Override
 protected T load()
 {
 return load(clazz, id);
 }

 protected abstract T load(Class clazz, Serializable id);
}
{% endhighlight %}

<ul>
<li>26: the method used to perform the actual loading of entity from the ORM</li>
</ul>
<p>This model can load any entity given an instance or a class and id, and will detach itself at the end of request. The model is abstract because the way to retrieve a handle to the ORM session varies from application to application. For example, in my project that uses <a href="http://salve.googlecode.com" target="_blank">Salve</a>&apos;s dependency injection and <a href="http://www.hibernate.org" target="_blank">Hibernate</a> a concrete implementation will look like this:</p>

{% highlight java linenos %}
public class EntityModel<T> extends AbstractEntityModel<T>
{
 @Dependency
 private Session session;

 @Override
 protected Identifiable load(Class clazz, Serializable id)
 {
 return session.get(clazz, id);
 }
}
{% endhighlight %}

<ul>
<li>3: Salve annotation that will make sure field session is injected with the current hibernate session</li>
<li>7-10: concrete implementation of the #load() method using hibernate Session</li>
</ul>
<h3>Handling Errors</h3>
<p>There are cases when a user deletes an object from the database that another user is currently viewing in the UI. In this case when the UI tries to render and calls getObject() on our EntityModel it will return null. There is nothing worse in the log then a cryptic NullPointerExcepton. A much better approach is to check for this condition and throw a type of exception that can be intercepted and processed appropriately by the UI.</p>

{% highlight java linenos %}
public abstract class AbstractEntityModel<T extends Identifiable< ? >>
 extends
 LoadableDetachableModel<T>
{
 
 @Override
 protected T load()
 {
 T entity = load(clazz, id);
 if (entity == null)
 {
 throw new EntityNotFoundException(clazz, id);
 }
 return entity;
 }

 }
{% endhighlight %}

<ul>
<li>10-13: built-in null check for better error reporting</li>
</ul>
<p>Then in our RequestCycle subclass:</p>

{% highlight java linenos %}
public class MyRequestCycle extends WebRequestCycle
{
 @Override
 public Page onRuntimeException(Page page, RuntimeException e)
 {
 if (e instanceof EntityNotFoundException)
 {
 return new EntityNotFoundErrorPage((EntityNotFoundException)e);
 }
 else
 {
 return super.onRuntimeException(page, e);
 }
 }

}
{% endhighlight %}

<p>EntityNotFoundErrorPage can present a user-friendly message explaining why the error happened.</p>
<h3>Using EntityModel to bind to Forms</h3>
<p>Suppose the following code:</p>

{% highlight java linenos %}
public class EditPersonPage extends WebPage
{
 public EditPersonPage(Long personId)
 {
 this(new EntityModel<Person>(Person.class, personId));
 }

 private EditPersonPage(IModel<Person> model)
 {

 Form<Person> form = new Form<Person>("form", new CompoundPropertyModel<Person>(model))
 {
 @Override
 public boolean process()
 {
 beginTransaction();
 if (super.process())
 {
 commitTransaction();
 return true;
 }
 else
 {
 rollbackTransaction();
 return false;
 }

 }
 };

 add(form);

 form.add(new TextField<String>("name"));
 }
}
{% endhighlight %}

<p>When the form is submitted the following series of calls will be made:</p>
<ul>
<li> Form#process()
<ul>
<li> beginTransaction()</li>
<li> super.proces()
<ul>
<li> Person p=EntityModel.getObject()</li>
<li> p.setName(name);</li>
<li> Form#onSubmit()</li>
</ul>
</li>
<li> commitTransaction();</li>
</ul>
</li>
<li> EntityModel#detach();</li>
</ul>
<p>This is interesting because the Person entity is loaded and the TextField sets the value on the loaded Person inside the same transaction, which means this value will be persisted automatically by the ORM when the transaction ends. Any changes done to this form"s data are automatically reflected in the database without any extra code.</p>
<p>Our AbstractEntityModel is already good enough to handle this scenario, but what if we wanted to reuse our form to create a  new entity instance instead of just editing one? If we had a way to do this we can then reuse any Form to create or edit an entity. To achieve this we need to add two things to our model:</p>
<ol>
<li>Ability to hold on to a transient entity instance (disable detaching while the entity is transient)</li>
<li> Allow the model to intelligently begin detaching if a transient entity instance has become persistent between getObject() and detach()</li>
</ol>
<p>Since it is not possible to override LoadableDetachableModel’s detach() we will have to convert our model to implement IModel directly – giving us full control:</p>

{% highlight java linenos %}
public abstract class AbstractEntityModel<T extends Identifiable< ? >> implements IModel<T>
{
 private final Class clazz;
 private Serializable id;

 private T entity;

 public AbstractEntityModel(T entity)
 {
 clazz = entity.getClass();
 id = entity.getId();
 this.entity = entity;
 }

 public AbstractEntityModel(Class< ? extends T> clazz, Serializable id)
 {
 this.clazz = clazz;
 this.id = id;
 }

 public T getObject()
 {
 if (entity == null)
 {
 if (id != null)
 {
 entity = load(clazz, id);
 if (entity == null)
 {
 throw new EntityNotFoundException(clazz, id);
 }
 }
 }
 return entity;
 }

 public void detach()
 {
 if (entity != null)
 {
 if (entity.getId() != null)
 {
 id = entity.getId();
 entity = null;
 }
 }
 }

 protected abstract T load(Class clazz, Serializable id);

 public void setObject(T object)
 {
 throw new UnsupportedOperationException(getClass() 
 " does not support #setObject(T entity)");
 }
}
{% endhighlight %}

<ul>
<li>6: cache for the entity instance that we keep for the request, only the first #getObject() call will populate it</li>
<li>12: notice we initialize the cache if we have the instance</li>
<li>25: guard so we only call #load() if we have a valid id</li>
<li>39: guard from preventing detaching a detached model</li>
<li>41: guard to make sure we do not null out a transient entity instance, we want to keep it until it is persisted</li>
<li>43: if a transient instance was persisted keep its id so it can be reloaded</li>
<li>44: now that we have a valid id we no longer need to keep the cache</li>
</ul>
<p>From now on our EntityModel will hold on to the transient entity until it is persisted. If we subclass the Form in the previous example and override its onSubmit() to persist the entity the model will switch into persistent mode after the form is submitted. So, it doesn"t matter if the EntityModel given to that form contains a transient or a persistent instance of the entity, the model and therefore also the form will simply do the right thing.</p>
<p>That’s it for now. Perhaps if there is more interest in such models I will revisit this and add a couple of other useful features.</p>
