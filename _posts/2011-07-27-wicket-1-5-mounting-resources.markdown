---
layout: post
status: publish
published: true
title: Wicket 1.5 - Mounting resources
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 644
wordpress_url: http://wicketinaction.com/?p=644
date: '2011-07-27 17:20:43 +0200'
date_gmt: '2011-07-27 15:20:43 +0200'
categories:
- Wicket
tags:
- Wicket
- '1.5'
- mapper
- resource
comments:
- id: 1083
  author: robmcguinness
  author_email: robert.mcguinness.iii@gmail.com
  author_url: ''
  date: '2011-07-30 11:53:45 +0200'
  date_gmt: '2011-07-30 09:53:45 +0200'
  content: fantastic tutorial.  thanks!
- id: 1117
  author: Marcin Zasepa
  author_email: sempa@vp.pl
  author_url: ''
  date: '2011-09-18 14:27:45 +0200'
  date_gmt: '2011-09-18 12:27:45 +0200'
  content: Is it not so that returning always new instance of ImageResource cause
    that it doesn't benefit from cache mechanism? How could mapping of image resources
    look like together with caching? Is there any suitable class for that or it is
    necessary to provide thread safe or session scoped ResourceReference implementation?  Greets,
    Marcin
- id: 1118
  author: martin-g
  author_email: martin.grigorov@gmail.com
  author_url: ''
  date: '2011-09-19 16:26:17 +0200'
  date_gmt: '2011-09-19 14:26:17 +0200'
  content: "If you specify caching headers with org.apache.wicket.request.resource.DynamicImageResource.configureResponse(ResourceResponse
    response, Attributes attrs) { response.setCacheDuration(Duration.valueOf(...))}
    then there wont be another request coming to the server side until the cache expires.\r\n\r\nThe
    ResourceReference is registered in the application scope so you must be careful
    to synchronize on its member fields. But if you keep it stateless (no member fields
    which are scoped by IoC manager) then there is no need to synchronize."
- id: 1167
  author: Wicket in meinem MoinMoin Wiki | Hotchpotch Blog
  author_email: ''
  author_url: http://hotchpotch-blog.de/2012/10/20/wicket-in-meinem-moinmoin-wiki/
  date: '2012-10-20 08:56:35 +0200'
  date_gmt: '2012-10-20 06:56:35 +0200'
  content: '[...] http://wicketinaction.com/2011/07/wicket-1-5-mounting-resources/
    [...]'
---
<p>In the previous <a href="http://wicketinaction.com/2011/07/wicket-1-5-mounting-pages/">article</a> I described how to mount pages at nice looking urls. Today I'll show you how to mount resource.</p>
<p>Recently there were several people asking in the mailing lists how to serve images loaded from a database so this will be the functionality of the demo application for this article. </p>
<p>To mount a resource you should do:<br />
MyApp.java</p>

{% highlight java %}
public void init() {
  super.init();
  mountResource("/mount/path", new SomeResourceReference());
  // #mountResource() is a shortcut for:
  // mount(new ResourceMapper("/mount/path", new com.example.SomeResourceReference()));
}{% endhighlight %}

<p>With this setup requests to <em>http://host/mount/path</em> will be handled by com.example.SomeResourceReference. The extraction of the request parameters is the same as with pages. A request to <em>http://host/mount/path/indexed?queryNamed=queryValue</em> will have one indexed and one named parameters. They can be read with <em title="org.apache.wicket.request.mapper.parameter.PageParameters">Attributes.getParameters()</em>.get(0) and .get("queryNamed") respectfully. Named indexed parameters also work the same way as with pages.</p>
<p>Now since we know how to mount resources let's see how to create links to them.</p>
<p>MyApp.java</p>

{% highlight java %}public void init() {
  super.init();
  mountResource("/images/${name}", new ImageResourceReference());
  mountPage("imagesPage", ImageResourcesPage.class);
}
{% endhighlight %}

<p>Here we mounted the resource reference that will find the images and a page which will make use of them.</p>
<p>The important thing when creating the resource reference is to give it unique <em title="org.apache.wicket.request.resource.ResourceReference.Key">ResourceReference.Key</em> and provide implementation of <em title="org.apache.wicket.request.resource.ResourceReference.getResource()">#getResource()</em> method:<br />
ImageResourceReference.java</p>

{% highlight java %}
public ImageResourceReference() {
    // this creates a Key with scope 'ImageResourceReference.class' 
    // and a name 'imagesDemo'
    super(ImageResourceReference.class, "imagesDemo");
}

@Override
public IResource getResource() {
    return new ImageResource();
}
{% endhighlight %}

<p>The key is needed by <em title="org.apache.wicket.request.mapper.ResourceMapper">ResourceMapper</em> to be able to recognize the resource reference when asked to create urls to it.<br />
ImageResource is an implementation of <em title="org.apache.wicket.request.resource.IResource">IResource</em> which will serve the images. </p>
<p>ImageResource.java:</p>

{% highlight java %}
 private static class ImageResource extends DynamicImageResource {

     @Override
     protected byte[] getImageData(Attributes attributes) {

         PageParameters parameters = attributes.getParameters();
         StringValue name = parameters.get("name");
            
         byte[] imageBytes = null;
            
         if (name.isEmpty() == false) {
             imageBytes = getImageAsBytes(name.toString());
         }
         return imageBytes;
     }

     private byte[] getImageAsBytes(String label) {...}

     @Override
     public boolean equals(Object that) {
         return that instanceof ImageResource;
     }
}
{% endhighlight %}

<p>Since we need to serve images we take advantage of Wicket's <em title="org.apache.wicket.request.resource.DynamicImageResource">DynamicImageResource</em> which cares about HTTP headers and stuff and leave us only to load the image's bytes. The parameter <em title="org.apache.wicket.request.resource.IResource.Attributes">Attributes</em> gives us access to the current request, response and the extracted request parameters. The override of #equals() is the second requirement needed by <em title="org.apache.wicket.request.mapper.ResourceMapper">ResourceMapper</em> to fully recognize the mapped resource. For our need any ImageResource instance will do the job.</p>
<p>To create urls to images you should use the following  code:<br />
ImageResourcesPage.java</p>

{% highlight java %}
 ResourceReference imagesResourceReference = new ImageResourceReference();
 PageParameters imageParameters = new PageParameters();
        
 String imageName = "anyName.jpg";
 imageParameters.set("name", imageName);
 CharSequence urlForImage = getRequestCycle().urlFor(imagesResourceReference, imageParameters);
 ExternalLink link = new ExternalLink("link", urlForImage.toString());
{% endhighlight %}

<p>The link will produce urls like /images/anyName.jpg.</p>
<p>With this the task is done.</p>
<p>The <a href="https://github.com/martin-g/blogs/tree/master/request-mappers/src/main/java/com/wicketinaction/requestmappers/resources/images">full source</a> of the demo application can be found in my Github account.</p>
