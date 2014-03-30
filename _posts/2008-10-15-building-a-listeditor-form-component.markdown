---
layout: post
status: publish
published: true
title: Building a ListEditor form component
author: ivaynberg
author_login: ivaynberg
author_email: igor.vaynberg@gmail.com
excerpt: "A common question on Wicket mailing lists is \"Why doesn't my <code>ListView</code>
  work as expected in a <code>Form</code>?\". There can be many possible answers,
  but the core issue is that <code>ListView</code> was never designed to work as a
  form component. <code>ListView</code> is great when it comes to displaying a list
  of elements, but in order to be a good citizen in Wicket's form processing a list
  editor should possess the following features:\r\n<ul>\r\n\t<li>Preserve component
  hierarchy that represents list items - this means that components the user added
  to represent an item in the list should be preserved even if list items are shuffled
  around. This will allow things like feedback messages and error indicators to work
  seamlessly and transparently</li>\r\n\t<li>Implement atomic form updates - this
  is perhaps the most important feature. If the user moves an item up or down in the
  list this change should not be reflected in the model object until the form is submitted.
  Same goes for actions such as item additions and removals.</li>\r\n<li>Should be
  reusable for different usecases</li>\r\n</ul>\r\n\r\nSurprisingly, implementing
  such a component is trivial. We will start by implementing the <code>ListEditor</code>
  itself and its one supporting class: <code>ListItem</code>, and later move on to
  implementing a Remove button. As always the fully functional sample project will
  be attached at the bottom.\r\n"
wordpress_id: 228
wordpress_url: http://wicketinaction.com/?p=228
date: '2008-10-15 17:40:11 +0200'
date_gmt: '2008-10-15 15:40:11 +0200'
categories:
- Wicket
tags:
- Wicket
- form
- listview
- repeater
- listeditor
comments:
- id: 54
  author: D
  author_email: ''
  author_url: http://blog.developpez.com/djo-mos?title=editeur_de_liste_wicket_igor_vaynberg
  date: '2008-10-15 18:57:27 +0200'
  date_gmt: '2008-10-15 16:57:27 +0200'
  content: '[...] http://wicketinaction.com/2008/10/building-a-listeditor-form-component/
    [...]'
- id: 55
  author: tom
  author_email: thomas.denouette@gmail.com
  author_url: ''
  date: '2008-10-17 11:33:41 +0200'
  date_gmt: '2008-10-17 09:33:41 +0200'
  content: "Example doesn't work because interface \"IFormModelUpdateListener\" is
    not a part of wicket.\r\n\r\nPlease post complete Example or don't...."
- id: 56
  author: dashorst
  author_email: martijn.dashorst@gmail.com
  author_url: http://martijndashorst.com
  date: '2008-10-17 12:19:36 +0200'
  date_gmt: '2008-10-17 10:19:36 +0200'
  content: Or you can just be grateful that someone actually writes articles. When
    was your last article written? Jeez.
- id: 58
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-17 18:25:22 +0200'
  date_gmt: '2008-10-17 16:25:22 +0200'
  content: '@tom: Forgot to mention the article was written against Wicket snapshot.
    I was hoping m4 would be released before I finished the article but it didn''t
    happen. You could have tried the latest snapshot yourself before posting...'
- id: 66
  author: Tetsuo
  author_email: me@mailinator.com
  author_url: ''
  date: '2008-10-19 04:11:20 +0200'
  date_gmt: '2008-10-19 02:11:20 +0200'
  content: "@ivaynberg: well, people don't usually expect to have to use the last
    development version to run examples in articles, unless explicitly stated :)\r\n\r\nAnyway,
    thank you for the tips! I'm just starting a project using Wicket, and probably
    will hit this problem sometime.\r\n\r\nobs.: I sure hope 1.4-m4 to be released
    soon... milestones are somewhat ok, but I'm not that confortable using snapshot
    versions... ^_^"
- id: 67
  author: Nino
  author_email: nino.martinez.wael@gmail.com
  author_url: http://ninomartinez.wordpress.com
  date: '2008-10-19 10:48:26 +0200'
  date_gmt: '2008-10-19 08:48:26 +0200'
  content: Keep em comming Igor.. Really nice reading the articles:)
- id: 410
  author: Frank
  author_email: krosagua@yahoo.com
  author_url: ''
  date: '2009-02-17 22:35:36 +0100'
  date_gmt: '2009-02-17 20:35:36 +0100'
  content: '@Igor: Very nice article. Right now in the downloadable app you set a
    CompoundPropertyModel to access a Phone''s variables. How would you set the model
    if instead of having a List you had a List? I can''t see how to plug in a dynamic
    model in the case of Strings. Do you mind sharing this? Thanks.'
- id: 411
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-02-17 23:06:26 +0100'
  date_gmt: '2009-02-17 21:06:26 +0100'
  content: '@Frank: Sorry frank, I dont undertand what you mean by "instead of having
    a List you had a List?"...'
- id: 412
  author: Frank
  author_email: krosagua@yahoo.com
  author_url: ''
  date: '2009-02-17 23:20:30 +0100'
  date_gmt: '2009-02-17 21:20:30 +0100'
  content: "@Igor: Oops, i meant instead of having a List you had a List (which, by
    the way, might be more common than having a List of an entity).\r\n Imagine your
    Phone wasn't composed of areacode + phone + ext, but only of a String, so that
    Person would have List that represent its phone numbers. Hope it's clear now!"
- id: 413
  author: Frank
  author_email: krosagua@yahoo.com
  author_url: ''
  date: '2009-02-17 23:23:17 +0100'
  date_gmt: '2009-02-17 21:23:17 +0100'
  content: "Hmmm... I knew I typed in correctly the first time, it's Wordpress who
    is stripping type parameters! I'll now try in scala-ish syntax:\r\n\r\n\"instead
    of having a List[Phone] you had a List[String]\""
- id: 414
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-02-17 23:44:58 +0100'
  date_gmt: '2009-02-17 21:44:58 +0100'
  content: '@Frank: Then I suppose you would add a single TextField whose model would
    be the same as the model of the listitem ( item.getModel() )'
- id: 418
  author: Frank
  author_email: krosagua@yahoo.com
  author_url: ''
  date: '2009-02-18 11:44:07 +0100'
  date_gmt: '2009-02-18 09:44:07 +0100'
  content: "@Igor: That was my first natural reaction, set the TextField's model to
    item.getModel() but it didn't work - it couldn't set the model.\r\n\r\nThis morning
    I found out it was pretty obvious: the ListItemModel was read-only. So I basically
    made ListItemModel implement IModel (not extend AbstractReadOnlyModel) and in
    the setModel I did:\r\n\r\npublic void setObject(T object) {\r\n\t\t\t((ListEditor)ListItem.this.getParent()).items.set(getIndex(),
    object);\r\n\r\n}\r\n\r\n...and it works like a charm. Thanks"
- id: 435
  author: keykubat
  author_email: keykuba@gmail.com
  author_url: ''
  date: '2009-02-28 22:19:17 +0100'
  date_gmt: '2009-02-28 20:19:17 +0100'
  content: "example does not work?\r\n\r\norg.apache.wicket.WicketRuntimeException:
    Unable to create application of class com.wicketinaction.WicketApplication\r\n\tat
    org.apache.wicket.protocol.http.ContextParamWebApplicationFactory.createApplication(ContextParamWebApplicationFactory.java:82)\r\n\tat
    org.apache.wicket.protocol.http.ContextParamWebApplicationFactory.createApplication(ContextParamWebApplicationFactory.java:49)\r\n\tat
    org.apache.wicket.protocol.http.WicketFilter.init(WicketFilter.java:677)\r\n\tat
    org.mortbay.jetty.servlet.FilterHolder.doStart(FilterHolder.java:99)\r\n\tat org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:40)\r\n\tat
    org.mortbay.jetty.servlet.ServletHandler.initialize(ServletHandler.java:594)\r\n\tat
    org.mortbay.jetty.servlet.Context.startContext(Context.java:139)\r\n\tat org.mortbay.jetty.webapp.WebAppContext.startContext(WebAppContext.java:1218)\r\n\tat
    org.mortbay.jetty.handler.ContextHandler.doStart(ContextHandler.java:500)\r\n\tat
    org.mortbay.jetty.webapp.WebAppContext.doStart(WebAppContext.java:448)\r\n\tat
    org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:40)\r\n\tat
    org.mortbay.jetty.handler.HandlerWrapper.doStart(HandlerWrapper.java:117)\r\n\tat
    org.mortbay.jetty.Server.doStart(Server.java:220)\r\n\tat org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:40)\r\n\tat
    runjettyrun.Bootstrap.main(Bootstrap.java:76)\r\n...."
- id: 464
  author: Rick
  author_email: r_mercallo@hotmail.com
  author_url: ''
  date: '2009-04-23 07:15:47 +0200'
  date_gmt: '2009-04-23 05:15:47 +0200'
  content: Thanks for this great article. This is exactly what I was looking for.
    Superb.
- id: 504
  author: L'engin
  author_email: wicket@leangen.net
  author_url: http://www.leangen.net
  date: '2009-06-06 10:25:02 +0200'
  date_gmt: '2009-06-06 08:25:02 +0200'
  content: "Great article!\r\n\r\nI was looking how to do this in 1.3.x, and it appears
    to be much more complicated due to final methods and coding against classes vs.
    interfaces.\r\n\r\nHow would you suggest one do this in older versions of Wicket?"
- id: 507
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-06-06 19:44:31 +0200'
  date_gmt: '2009-06-06 17:44:31 +0200'
  content: "The only piece that is missing in 1.3.x is the IFormModelUpdateListener,
    so you will have to manually invoke updateModel() on the ListEditor. There are
    many options to do this:\n\n\tImplement IFormModelUpdateListener yourself and
    call it from a custom form's overridden process() method.\n\tPut the ListEditor
    into a FormComponentPanel and call ListEditor.updateModel() from FCP.updateModel()\n\tAdd
    a HiddenField to the form and call ListEditor.updateModel() from HiddenField.updateModel()\n\n\nOther
    then that everything else should be portable I believe."
- id: 509
  author: L'engin
  author_email: wicket@leangen.net
  author_url: http://www.leangen.net
  date: '2009-06-08 10:04:27 +0200'
  date_gmt: '2009-06-08 08:04:27 +0200'
  content: "Ok, cool!\r\n\r\nI used the FormComponentPanel method you suggested. It
    took a bit (ok a lot) of fumbling with details to get it to work, but now that
    it's done, works like a charm. Also, other than a few things (like have to use
    getParent().iterator() instead of getParent().get(int) in the RemoveButton), most
    things are portable as you say.\r\n\r\nThanks!"
- id: 545
  author: garz
  author_email: garz@gmx.net
  author_url: ''
  date: '2009-07-09 17:43:37 +0200'
  date_gmt: '2009-07-09 15:43:37 +0200'
  content: "hi ivaynberg,\r\n\r\nat first i want to thank you for this article. i
    have the book \"wicket in action\" and there it isnt explained the same way as
    it is here and i have the feeling, this solution is nicer than the one in the
    book. thank you very much!\r\n\r\ni have one question. in the ListEditor-class
    in the method onBeforeRender() you set the items-attribute like this:\r\nitems
    = new ArrayList(getModelObject());\r\nwhy not doing it like this:\r\nitems = getModelObject();\r\nwhy
    do you create a new ArrayList-object? is it because the constructor supports the
    more general interface Collection? if so, why not using Collection as the type
    for the items-attribute?\r\n\r\njust asking because i dont want to miss something.
    maybe there is something i dont understand.\r\n\r\nthank you and keep the good
    work up. :)"
- id: 546
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-07-10 06:14:20 +0200'
  date_gmt: '2009-07-10 04:14:20 +0200'
  content: "@garz: Because we want all changes to the list to be comitted atomically
    when the form is submitted we make a copy of the actual list backing the model.\r\n\r\nIf
    we did not do this then operations such as move up and move down, for example,
    would immediately affect the model object before the form was submitted."
- id: 553
  author: John
  author_email: bongo_john_uk@yahoo.co.uk
  author_url: http://jdiligence.com
  date: '2009-07-24 17:39:03 +0200'
  date_gmt: '2009-07-24 15:39:03 +0200'
  content: "I had this working fine when everything was on a single page. I also used
    the above example to make it work with lists of strings.\r\n\r\nThen I separated
    different parts of the form into panels on tabs. Now that it is in a panel the
    add button does not update its model so always adds an empty string.\r\n\r\nI
    don't see why it should behave differently. Any ideas?\r\n\r\nThe hierachy was..\r\nform-&gt;listeditor
    stuff\r\n\r\nThe hierarchy is now..\r\nform-&gt;tabbedpanel-&gt;panel-&gt;listeditor
    stuff\r\n\r\nMaybe there is another problem with forms separated into panels but
    wizards do this fine..\r\n\r\nHelp!"
- id: 556
  author: John
  author_email: bongo_john_uk@yahoo.co.uk
  author_url: http://jdiligence.com
  date: '2009-07-25 17:48:35 +0200'
  date_gmt: '2009-07-25 15:48:35 +0200'
  content: Also, now when I switch tabs any updates get lost unless I have submitted
    the whole form. This is not very intuitive for the user as the tabs really just
    split up a very very long form which has many parts that are optional.
- id: 561
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2009-07-30 18:59:51 +0200'
  date_gmt: '2009-07-30 16:59:51 +0200'
  content: |-
    @John: Remember, the whole point of the ListEditor was that submit is atomic. The problem, I would guess, is that you use a regular TabbedPanel which uses a regular anchor link to switch tabs. That means the form is not submitted.

    There are two ways to solve it:

    a) Override tabbedpanel's newLink() factory and return a SubmitLink instead - this will submit the form between tab switches.

    b) Use a javascript tab panel instead so that everything is still kept on the same page.
- id: 593
  author: sam
  author_email: sam@sambarrow.com
  author_url: ''
  date: '2009-09-24 08:18:27 +0200'
  date_gmt: '2009-09-24 06:18:27 +0200'
  content: |-
    I'm getting this error:

    java.lang.UnsupportedOperationException: Model class x.x.x.listeditor.ListEditorItem$ListItemModel does not support setObject(Object)
    at org.apache.wicket.model.AbstractReadOnlyModel.setObject(AbstractReadOnlyModel.java:55)
         at org.apache.wicket.Component.setDefaultModelObject(Component.java:3052)
         at org.apache.wicket.markup.html.form.FormComponent.updateModel(FormComponent.java:1168)
- id: 666
  author: James
  author_email: gymper@gmail.com
  author_url: ''
  date: '2010-03-17 18:59:01 +0100'
  date_gmt: '2010-03-17 16:59:01 +0100'
  content: "Great article - is there a way to modify it to display the items without
    text boxes until you click on them, kind of like an example you have in the book?\r\nThanks,\r\n-jim"
- id: 967
  author: amango
  author_email: tang_heidi@yahoo.com
  author_url: ''
  date: '2011-05-17 08:51:37 +0200'
  date_gmt: '2011-05-17 06:51:37 +0200'
  content: i wonder how easy is it to make the add and remove buttons do Ajax call?
    i have other controls on top of the page, and every time after add/remove, it
    goes back to the top of the page.
- id: 968
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2011-05-17 17:38:45 +0200'
  date_gmt: '2011-05-17 15:38:45 +0200'
  content: '@amango, its actually pretty easy - see here: http://wicketinaction.com/2008/10/repainting-only-newly-created-repeater-items-via-ajax/'
---
<p>A common question on Wicket mailing lists is "Why doesn't my <code>ListView</code> work as expected in a <code>Form</code>?". There can be many possible answers, but the core issue is that <code>ListView</code> was never designed to work as a form component. <code>ListView</code> is great when it comes to displaying a list of elements, but in order to be a good citizen in Wicket's form processing a list editor should possess the following features:</p>
<ul>
<li>Preserve component hierarchy that represents list items - this means that components the user added to represent an item in the list should be preserved even if list items are shuffled around. This will allow things like feedback messages and error indicators to work seamlessly and transparently</li>
<li>Implement atomic form updates - this is perhaps the most important feature. If the user moves an item up or down in the list this change should not be reflected in the model object until the form is submitted. Same goes for actions such as item additions and removals.</li>
<li>Should be reusable for different usecases</li>
</ul>
<p>Surprisingly, implementing such a component is trivial. We will start by implementing the <code>ListEditor</code> itself and its one supporting class: <code>ListItem</code>, and later move on to implementing a Remove button. As always the fully functional sample project will be attached at the bottom.<br />
<a id="more"></a><a id="more-228"></a><br />
Lets begin with the source to the <code>ListEditor</code> class:</p>

{% highlight java linenos %}
public abstract class ListEditor<T> extends RepeatingView 
                             implements IFormModelUpdateListener
{
    List<T> items;

    public ListEditor(String id, IModel<List<T>> model)
    {
        super(id, model);
    }

    protected abstract void onPopulateItem(ListItem<T> item);

    public void addItem(T value)
    {
        items.add(value);
        ListItem<T> item = new ListItem<T>(newChildId(), 
                                              items.size() - 1);
        add(item);
        onPopulateItem(item);
    }

    protected void onBeforeRender()
    {
        if (!hasBeenRendered())
        {
            items = new ArrayList<T>(getModelObject());
            for (int i = 0; i < items.size(); i++)
            {
                ListItem<T> li = new ListItem<T>(newChildId(), i);
                add(li);
                onPopulateItem(li);
            }
        }
        super.onBeforeRender();
    }

    public void updateModel()
    {
        setModelObject(items);
    }
}
{% endhighlight %}

<p>As you can see there is not much to it.</p>
<p>First there is the familiar <code>onPopulateItem</code> callback which we provide to allow the user to populate rows of the list editor with form components. Because we want to be in complete control over the lifecycle of those components we use <code>RepeatingView</code> as the base class. <code>RepeatingView</code> is the lowest level repeater and does not provide any automatic item management like <code>ListView</code> and other higher-level repeaters.</p>
<p>We want to initialize <code>ListEditor</code> to the state of the model when it first shows up, so before it is rendered for the first time we build the component hierarchy to represent the initial state of the model object list in <code>onBeforeRender</code>.</p>
<p>The last interesting tidbit is the items collection and <code>IFormModelUpdateListener#updateModel()</code>. Because we want updates to model object to be atomic we copy the initial state of the model object into the items list and sync it back in <code>updateModel()</code> which is called only when the form's model is being updated (ie the type conversion and validation steps have passed for all form components the model contains).</p>
<p>The only missing piece is the <code>ListItem</code> class used in the code above. This is a standard repeater item that provides the added convenience of binding its model to an item at a certain index in the <code>ListEditor#items</code> collection:</p>

{% highlight java linenos %}
public class ListItem<T> extends Item<T>
{
   public ListItem(String id, int index)
   {
      super(id, index);
      setModel(new ListItemModel());
   }

   private class ListItemModel extends AbstractReadOnlyModel<T>
   {
      public T getObject()
      {
         return ((ListEditor<T>)ListItem.this.getParent())
                  .items.get(getIndex());
      }
   }
}
{% endhighlight %}

<p>At this point we have a pretty good replacement for the <code>ListView</code> with a bonus that our <code>ListEditor</code> has an almost exact same usage pattern as the <code>ListView</code>. The minor difference being that items are added to the <code>ListEditor</code> by using <code>ListEditor#addItem()</code> instead of adding them directly to the model object list - this is, again, so that the model object list  is not modified until the form is submitted. An Add button can be implemented like so:</p>

{% highlight java linenos %}
 form.add(new Button("add")
        {
            public void onSubmit()
            {
                listEditor.addItem(new Phone());
            }
        }.setDefaultFormProcessing(false));
{% endhighlight %}

<p>The only lesson to take away from here is to call <code>Button#setDefaultFormProcessing(false)</code> on the add button so that the form is not processed since we are not updating the models.</p>
<p>Now to implement a Remove button which will remove rows from the editor without submitting the form, but first a convenience base class for buttons meant to be used inside the <code>ListEditor</code>:</p>

{% highlight java linenos %}
public abstract class EditorButton extends Button
{
    private transient ListItem< ? > parent;

    public EditorButton(String id)
    {
        super(id);
    }

    protected final ListItem< ? > getItem()
    {
        if (parent == null)
        {
            parent = findParent(ListItem.class);
        }
        return parent;
    }

    protected final List< ? > getList()
    {
        return getEditor().items;
    }

    protected final ListEditor< ? > getEditor()
    {
        return (ListEditor< ? >)getItem().getParent();
    }

    protected void onDetach()
    {
        parent = null;
        super.onDetach();
    }
{% endhighlight %}

<p>This class will allow us to find the <code>ListEditor</code> instance instead of having to pass it into the button. And now, the button itself:</p>

{% highlight java linenos %}
public class RemoveButton extends EditorButton
{

    public RemoveButton(String id)
    {
        super(id);
        setDefaultFormProcessing(false);
    }

    @Override
    public void onSubmit()
    {
        int idx = getItem().getIndex();

        for (int i = idx + 1; i < getItem().getParent().size(); i++)
        {
            ListItem< ? > item = (ListItem< ? >)getItem().getParent().get(i);
            item.setIndex(item.getIndex() - 1);
        }

        getList().remove(idx);
        getEditor().remove(getItem());
    }
}
{% endhighlight %}

<p>The only trick here is that we have manually decremented the index property of <code>ListItem</code>s affected by the removal. We have to do this we are reusing the preserving the component hierarchy and need to keep it in sync with the underlying model.</p>
<p>We now have a fully functional, if not yet fully featured, <code>ListEditor</code> that will work much better then <code>ListView</code> ever could. In the next article we will create buttons that will move rows up and down as well as some handy utilities.</p>
<p>Sample project: <a href='http://wicketinaction.com/wp-content/uploads/2008/10/listeditor.zip'>listeditor</a> (requires at least WICKET-1.4m4 or snapshot)</p>
