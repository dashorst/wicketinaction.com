---
layout: post
status: publish
published: true
title: Creating pluggable applications with Wicket and Spring
author: ivaynberg
author_login: ivaynberg
author_email: igor.vaynberg@gmail.com
excerpt: "In the application I am currently working on it is quiet common to have
  plugins which affect the UI in different ways. Because of Wicket\"s component and
  OO nature it is very easy to build such functionality. In this article we are going
  to build a simple application that allows the user to create an article and configure
  how it should be deployed. Out of the box we are going to support a fictional FTP
  and HTTP POST deployers, each with their own configuration data and editors. We
  are going to abstract the deployer into a plugin system so new ones can be easily
  created and added to the application. The sample project provided works with Hibernate,
  Spring, and Wicket. In the end we will end up with something like the screenshots
  below. Notice that the UI is different based on which deployer type is selected
  in the dropdown...\r\n<div style=\"display: none\"></div>\r\n<img class=\"alignnone
  size-full wp-image-195\" src=\"http://wicketinaction.com/wp-content/uploads/2008/10/croppercapture1.jpg\"
  alt=\"\" width=\"330\" height=\"326\" />\r\n<img class=\"alignnone size-full wp-image-195\"
  src=\"http://wicketinaction.com/wp-content/uploads/2008/10/croppercapture2.jpg\"
  alt=\"\" width=\"330\" height=\"326\" />\r\n"
wordpress_id: 206
wordpress_url: http://wicketinaction.com/?p=206
date: '2008-10-07 04:52:00 +0200'
date_gmt: '2008-10-07 02:52:00 +0200'
categories:
- Wicket
tags:
- Wicket
- spring
- hibernate
- plugins
comments:
- id: 29
  author: Ray Krueger
  author_email: raykrueger@gmail.com
  author_url: http://raykrueger.blogspot.com/
  date: '2008-10-07 13:10:47 +0200'
  date_gmt: '2008-10-07 11:10:47 +0200'
  content: "Very cool. \r\nThough you could clean up your IdentifiableBeanRegistry
    class by implementing BeanPostProcessor rather than InitializingBean.\r\n\r\nUsing
    IntializingBean.afterPropertiesSet you're forced to go search the BeanFactory
    after the fact. By implementing BeanPostProcessor.postProcessAfterInitialization
    Spring will hand each bean to you as it creates them and you can simply use beanType.isAssignableFrom(bean.getClass())
    as they come in.\r\n\r\nHave a look at the BeanFactory lifecycle...\r\nhttp://static.springframework.org/spring/docs/2.0.x/api/org/springframework/beans/factory/BeanFactory.html"
- id: 30
  author: Per Ejeklint
  author_email: ejeklint@mac.com
  author_url: ''
  date: '2008-10-07 16:04:41 +0200'
  date_gmt: '2008-10-07 14:04:41 +0200'
  content: Great article! It solves a problem that's been nagging me for a while -
    how to write UI plugs for corresponding measuring and control hw devices. Much
    appreciated.
- id: 31
  author: Jonathan Doklovic
  author_email: nospam@sysbliss.com
  author_url: http://blog.sysbliss.com
  date: '2008-10-07 16:34:29 +0200'
  date_gmt: '2008-10-07 14:34:29 +0200'
  content: "Nice article, but I think it would be a little better if osgi was used
    instead of rolling your own plugin registry.\r\n\r\nShouldn't be that hard now
    that spring supports osgi.\r\nhttp://www.springframework.org/osgi\r\n\r\n- J"
- id: 32
  author: ivaynberg
  author_email: igor.vaynberg@gmail.com
  author_url: http://
  date: '2008-10-07 18:45:09 +0200'
  date_gmt: '2008-10-07 16:45:09 +0200'
  content: "@Ray: While BeanPostProcessors are definitely slicker they also come with
    shortcommings for this particular usecase. Beans that use AOP-autoproxying cannot
    be processed by postprocessors, so those plugins will not be registered. Also,
    when you create proxies or apply any kind of aop (eg @Transactional on a bean)
    both the real class and the cglib generated subclass are viewed by postprocessors
    so you would have to filter for duplicates. I find that my way, while not as elegant,
    simply works.\r\n\r\n@Per: Good to hear, enjoy.\r\n\r\n@Jonathan: This kind of
    thing can be done with any IOC container that allows introspection. The point
    of this article was not really to glue Wicket to Spring, but more to demonstrate
    the pattern. In this whole article there is only one Spring-related class - the
    registry. I had a template Wicket/Spring project setup and so it was easy for
    me to base this article on that. Of course, feel free to rewrite the attached
    code using OSGi and I will be happy to append it to the article."
- id: 437
  author: Ray A.
  author_email: raymond@interface101.com
  author_url: http://www.interface101.org
  date: '2009-03-02 19:13:51 +0100'
  date_gmt: '2009-03-02 17:13:51 +0100'
  content: Very nice framework. I have a maven script that will build a spring hibernate
    wicket template and separate the HTML page. It helps a lot between the designer
    "Front End" and core developer "Java/J2EE".
- id: 586
  author: dimitris
  author_email: dimitris@itconsulting.gr
  author_url: ''
  date: '2009-09-01 16:32:19 +0200'
  date_gmt: '2009-09-01 14:32:19 +0200'
  content: "Very nice framework.\r\n\r\nI was trying to amplify it to be able to also
    edit instances (Articles) with an edit button on each line, but I'm having the
    following problem:\r\n\r\nI managed to populate the correct data on the editArticlePage
    and if I change the DeployerPlugin the page updates correctly on selectionChange(),
    but on submit the Article still has the old DeployerPlugin value and gives an
    error. If you are interested I can send my changes until this point. If I find
    a solution I will update."
- id: 591
  author: Golfman
  author_email: chrisc@stepahead.com.au
  author_url: ''
  date: '2009-09-23 07:02:33 +0200'
  date_gmt: '2009-09-23 05:02:33 +0200'
  content: |-
    Beans, Spring, annotations - sounds like we've all lost our way!

    Whatever happened to simple POJOs with a simple lightweight IOC framework that you could put together yourself in an afternoon?

    Wicket, on the other hand, rocks massively!
- id: 598
  author: danisevsky
  author_email: danisevsky@gmail.com
  author_url: ''
  date: '2009-10-01 16:56:31 +0200'
  date_gmt: '2009-10-01 14:56:31 +0200'
  content: Great article, thank you Igor!
---
<p>In the application I am currently working on it is quiet common to have plugins which affect the UI in different ways. Because of Wicket"s component and OO nature it is very easy to build such functionality. In this article we are going to build a simple application that allows the user to create an article and configure how it should be deployed. Out of the box we are going to support a fictional FTP and HTTP POST deployers, each with their own configuration data and editors. We are going to abstract the deployer into a plugin system so new ones can be easily created and added to the application. The sample project provided works with Hibernate, Spring, and Wicket. In the end we will end up with something like the screenshots below. Notice that the UI is different based on which deployer type is selected in the dropdown...</p>
<div style="display: none"></div>
<p><img class="alignnone size-full wp-image-195" src="http://wicketinaction.com/wp-content/uploads/2008/10/croppercapture1.jpg" alt="" width="330" height="326" /><br />
<img class="alignnone size-full wp-image-195" src="http://wicketinaction.com/wp-content/uploads/2008/10/croppercapture2.jpg" alt="" width="330" height="326" /><br />
<a id="more"></a><a id="more-206"></a></p>
<h3>The Data Model</h3>
<p>Lets start by looking at the datamodel. The model is made up of two classes: the Article and the DeplployerConfig. The Article holds our main data, while DeployerConfig acts as a base class for the different deployer configurations: (notice JPA annotations are stripped)</p>

{% highlight java linenos %}
public class Article implements Serializable, Identifiable<Long>
{
 public Long id;
 public String title;
 public String content;
 public Date created;
 public DeployerConfig deployerConfig;
}
{% endhighlight %}

<p>and the DeployerConfig base class:</p>

{% highlight java linenos %}
public abstract class DeployerConfig implements Identifiable<Long>, Serializable
{
 public Long id;
 public String pluginId;
 public Article article;
}
{% endhighlight %}

<h3>The Plugin System</h3>
<p>In order to have a working plugin system we need three things:</p>
<ul>
<li>A common interface or base class for plugins to implement to give them structure</li>
<li>A way to identify a unique plugin and tie it to the data model</li>
<li>A way to discover all plugins</li>
</ul>
<p>Lets start by defining our plugin interface:</p>

{% highlight java linenos %}
public interface DeployerPlugin extends Identifiable<String>
{
 String getName();
 String deploy(Article article);
 Component newEditor(String id, IModel< ? extends DeployerConfig> model);
 DeployerConfig newConfig();
}
{% endhighlight %}

<p>Our plugin interface extends Identifiable<String> which means it has a String getId() method which we will use to return a uuid that will identifiy the plugin.</p>
<p>All we are missing is a way to discover all plugins in the system. Since we are using Spring for the purpose of this article we can build a simple registry bean to aid in plugin discovery:</p>

{% highlight java linenos %}
public class IdentifiableBeanRegistry<T extends Identifiable<ID>, ID extends Serializable>
 implements
 ApplicationContextAware,
 InitializingBean
{
 private final Class<T> beanType;

 private ApplicationContext context;
 private Map<ID, T> entries;
 private List<T> beans;
 private List<ID> uuids;

 public IdentifiableBeanRegistry(Class<T> beanType)
 {
 this.beanType = beanType;
 }

 public void setApplicationContext(ApplicationContext context) throws BeansException
 {
 this.context = context;
 }

 public T getEntry(Serializable id)
 {
 T entry = entries.get(id);
 if (entry == null)
 {
 throw new IllegalStateException();
 }
 return entry;
 }

 public List<T> getEntries()
 {
 return beans;
 }

 public List<ID> getIds()
 {
 return uuids;
 }

 @SuppressWarnings("unchecked")
 public void afterPropertiesSet() throws Exception
 {
 // initialize the registry
 entries = new HashMap<ID, T>();

 final Map<String, ? extends T> matches = BeanFactoryUtils.beansOfTypeIncludingAncestors(
 context, beanType);
 for (T entry : matches.values())
 {
 entries.put(entry.getId(), entry);
 }

 entries = Collections.unmodifiableMap(entries);

 // intialize indexes
 beans = new ArrayList<T>(entries.values());
 beans = Collections.unmodifiableList(beans);
 uuids = new ArrayList<ID>(entries.keySet());
 uuids = Collections.unmodifiableList(uuids);
 }
}
{% endhighlight %}
<p>The registry uses <a href="http://www.phpaide.com/download.php?langue=fr&id=12/">http://www.phpaide.com/download.php?langue=fr&id=12</a> spring"s context introspection to lookup all the beans that implement our plugin interface and caches it in the map. The registry is optimized for three basic operations:</p>
<ul>
<li>Listing all available plugins</li>
<li>Listing all available plugin uuids</li>
<li>Looking up a plugin by its uuid</li>
</ul>
<h3>The UI</h3>
<p>Armed with all this we are now ready to build the user interface. First we begin with a simple page to edit an Article:</p>
{% highlight java linenos %}
public abstract class EditArticlePage extends BasePage
{
 public EditArticlePage(IModel<Article> article)
 {
 add(new FeedbackPanel("feedback"));
 Form<Article> form = new TransactionalForm<Article>("form",
 new CompoundPropertyModel<Article>(article))
 {
 @Override
 protected void onSubmit()
 {
 onSave(getModelObject());
 }
 };
 add(form);
 form.add(new TextField<String>("title"));
 form.add(new TextArea<String>("content"));
 form.add(new Link<Void>("cancel")
 {
 @Override
 public void onClick()
 {
 onCancel();
 }
 });
 }
}
{% endhighlight %}

<p>Now we need a way to let the user select which deployer to use for this article and present selected deployer"s interface. For this we build a simple panel which will contain a dropdown that lets the user select a deployer and a swappable panel into which we will put selected deployer"s editor component:</p>

{% highlight java linenos %}
public class DeployerPluginSelector extends Panel
{
 @SpringBean
 private DeployersRegistry registry;

 public DeployerPluginSelector(String id, final IModel<Article> article)
 {
 super(id);
 add(new WebMarkupContainer("editor"));
 add(new PluginPicker("picker")
 {
 @Override
 protected void onSelectionChanged(String pluginId)
 {
 DeployerPlugin plugin = registry.getEntry(pluginId);
 article.getObject().setDeployerConfig(plugin.newConfig());

 DeployerPluginSelector.this.replace(plugin.newEditor("editor",
 new PropertyModel<DeployerConfig>(article, "deployerConfig")));
 }
 });
 }

 private static abstract class PluginPicker extends DropDownChoice<String>
 {
 @SpringBean
 private DeployersRegistry registry;

 public PluginPicker(String id)
 {
 super(id);
 setRequired(true);
 setModel(new Model<String>());
 setChoices(new LoadableDetachableModel<List< ? extends String>>()
 {
 @Override
 protected List< ? extends String> load()
 {
 return registry.getIds();
 }
 });
 setChoiceRenderer(new IChoiceRenderer<String>()
 {
 public Object getDisplayValue(String object)
 {
 return registry.getEntry(object).getName();
 }

 public String getIdValue(String object, int index)
 {
 return object;
 }
 });
 }

 @Override
 protected boolean wantOnSelectionChangedNotifications()
 {
 return true;
 }

 @Override
 protected abstract void onSelectionChanged(String pluginId);
 }

}
{% endhighlight %}
<p>A key part of what this picker does is create a new DeployerConfig based on selected plugin and binds it to the article, this way the plugin"s editor component gets the expected model object.</p>
<h3>Implementing a Plugin</h3>
<p>All thats left is to implement a couple of plugins and drop them into Spring context where they can be discovered by our registry. Lets implement a fictional HTTP POST plugin whose only configuration parameter is the URL to which it will POST the article. The plugin implementation consists of three parts:</p>
<ul>
<li>Implementation of the DeployerPlugin interface</li>
<li>Implementation of data structure the plugin will use to store its configuration</li>
<li>A Wicket component used to edit the configuration</li>
</ul>

{% highlight java linenos %}
public class HttpPostDeployerPlugin implements DeployerPlugin
{
 public static final String ID = "plugin.deployer.httppost";
 public String getName()
 {
 return "Http Post";
 }
 public String getId()
 {
 return ID;
 }
 public Component newEditor(String id, IModel< ? extends DeployerConfig> model)
 {
 return new HttpPostDeployerEditor(id, model);
 }
 public DeployerConfig newConfig()
 {
 return new HttpPostDeployerConfig();
 }
 public String deploy(Article article)
 {
 HttpPostDeployerConfig config = (HttpPostDeployerConfig)article.deployerConfig;
 return "Deploying article: " article.title " to http://" config.url;
 }
}
{% endhighlight %}

{% highlight java linenos %}
public class HttpPostDeployerConfig extends DeployerConfig
{
 public HttpPostDeployerConfig()
 {
 pluginId = FtpDeployerPlugin.ID;
 }

 public String url;
}
{% endhighlight %}

{% highlight java linenos %}
public class HttpPostDeployerEditor extends Panel
{
 public HttpPostDeployerEditor(String id, IModel< ? extends DeployerConfig> model)
 {
 super(id, new CompoundPropertyModel<DeployerConfig>(model));
 add(new TextField<String>("url").setRequired(true));
 }
}
{% endhighlight %}

<p>This is an example of a pretty functional plugin system in a web application. Thanks to Wicket it was pretty easy to create a UI that supports dynamic contributions from plugins. Oh yeah, it would be nice to see the article deployed using our plugin. Here is an implementation of Link that can do this:</p>

{% highlight java linenos %}
private static class DeployArticleLink extends Link<Article>
{
 @SpringBean
 private DeployersRegistry registry;

 private DeployArticleLink(String id, IModel<Article> model)
 {
 super(id, model);
 }

 @Override
 public void onClick()
 {
 Article article = getModelObject();
 DeployerPlugin plugin = registry.getEntry(article.deployerConfig.pluginId);
 info(plugin.deploy(article));
 }
}
{% endhighlight %}

<p>The fully functional example of this system implemented with Hibernate, Spring, and Wicket can be found below. It is a standard Wicket quickstart setup complete with Start class that can be used to launch the application from within an IDE.</p>
<p><a href="http://wicketinaction.com/wp-content/uploads/2008/10/plugin-article.zip">plugin-article.zip</a>
<div style="display: none">zp8497586rq</div>
