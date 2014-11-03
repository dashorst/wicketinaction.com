---
layout: post
status: publish
published: true
title: Uploading files to Wicket IResource
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 809
wordpress_url: http://wicketinaction.com/?p=809
date: '2012-11-14 11:40:35 +0100'
date_gmt: '2012-11-14 09:40:35 +0100'
categories:
- Wicket
tags: []
comments: []
---
<p>Several times people have asked in the mailing lists how to integrate JavaScript widgets for file uploading like <a href="http://www.plupload.com/">Plupload</a>, <a href="http://www.uploadify.com/">Uploadify</a>, <a href="http://fineuploader.com/index.html">Fine Uploader</a> or <a href="http://blueimp.github.com/jQuery-File-Upload/">jQuery-File-Upload</a> with Apache Wicket.</p>
<h2>The problem</h2>
<p>Wicket provides <em title="org.apache.wicket.markup.html.form.upload.FileUploadField">FileUploadField</em> which can be used as part of a form submission. It supports HTML5 <em>multiple</em> attribute and works great but these JavaScript widgets are smarter than that!</p>
<p>The widgets provide functionalities to select the files to upload with HTML5's Drag & Drop, or if HTML5 is not supported by the browser then fallback to Flash or IFrame solution.</p>
<p>So the widgets don't really need a form to be able to do their work. All they need is a destination URL to send the bytes to.</p>
<h2>The solution</h2>
<p>Users of Apache Wicket know very well its <em title="org.apache.wicket.markup.html.WebPage">WebPage</em> component. It is used as a main container of other smaller components that construct the content that is delivered to the client when a given url is requested.<br />
Since file uploading doesn't really need to construct a page and its optional response often is just a piece of data explaining that the upload is successful or not we are going to use Wicket's <em title="org.apache.wicket.request.resource.IResource">IResource</em> as an endpoint. IResource could be mounted at a predefined mount path just like pages by using a <em title="org.apache.wicket.request.resource.ResourceReference">ResourceReference</em>:     </p>
<p>MyApplication.java:</p>

{% highlight java %}

@Override
public void init() {
    super.init();

    mountResource("fileUpload", new FileUploadResourceReference(BASE_FOLDER));
    mountResource("fileManager", new FileManageResourceReference(BASE_FOLDER));
}

{% endhighlight %}

<p>Here we mounted two resources - one as destination for the file uploads and another for managing the uploaded files (it can serve and delete them).</p>
<p>Check <a href="http://wicketinaction.com/2011/07/wicket-1-5-mounting-resources/">Wicket 1.5 - Mounting resources</a> for more information.</p>
<h3>Reading the uploaded files</h3>
<p>The main problem to solve is how to read the uploaded files from the request parameters.<br />
Actually the solution is quite simple, just few lines of code:</p>
<p>AbstractFileUploadResource.java</p>

{% highlight java %}
@Override
protected ResourceResponse newResourceResponse(Attributes attributes)
{
	ServletWebRequest webRequest = (ServletWebRequest) attributes.getRequest();
	MultipartServletWebRequest multiPartRequest = webRequest.newMultipartWebRequest(getMaxSize(), "ignored");
	// Note: since Wicket 6.18.0 you will need to call "multiPartRequest.parseFileParts();" additionally here
	Map<String, List<FileItem>> files = multiPartRequest.getFiles();
	...
}
{% endhighlight %}

<p>We just asked Wicket to create a WebRequest that knows how to read the multipart request parameters. The final result is a map with keys the name of the request parameter and values the respective <em title="org.apache.wicket.util.upload.FileItem">FileItem</em>s which can be used to get the uploaded file's name, size, input stream, etc.</p>
<p>And basically that's it!<br />
From here on our code is responsible to store somewhere these files and return a response to the client is the format it expects - JSON, HTML, ...</p>
<h2>Demo</h2>
<p>At <a href="https://github.com/martin-g/blogs/tree/master/file-upload">my GitHub account</a> you may find a demo application that integrates <a href="http://blueimp.github.com/jQuery-File-Upload/">jQuery-File-Upload</a>. I've chosen this widget because it looks very nice and I wanted to contribute it to <a href="https://github.com/l0rdn1kk0n/wicket-bootstrap">Wicket Bootstrap</a> project. It appeared that the docs and APIs of this widget are not the best one could wish for. To be able to duplicate the functionalities from its demo page I had to read and debug its internals (specifically jquery.fileupload-ui.js).</p>
<p>There are still some functionalities which do not work completely:</p>
<ul>
<li> the cancel button</li>
<li> the gallery preview of the uploaded files</li>
<li> the UI is broken in IE (I blame my CSS skills for that!)</li>
<li> ... </li>
</ul>
<p>But all these are not directly related to the topic of this post - how to upload files to Wicket IResource and how to read them at the server side.<br />
If you need this component for your application and you fix this issues I'll gladly accept your Pull Requests.</p>
