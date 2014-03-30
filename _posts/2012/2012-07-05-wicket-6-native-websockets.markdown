---
layout: post
status: publish
published: true
title: Wicket 6 Native WebSockets
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 783
wordpress_url: http://wicketinaction.com/?p=783
date: '2012-07-05 16:48:16 +0200'
date_gmt: '2012-07-05 14:48:16 +0200'
categories:
- Wicket
tags: []
comments: []
---
<h2>Asynchronous communication with Web Sockets</h2>
<p>Wicket 6.0 introduces two new modules, both of them related to Server Push technology, Wicket-Atmosphere and Wicket Native WebSockets. In this article we will see how to make your application more dynamic by employing Wicket Native WebSockets.</p>
<h2>Web Sockets</h2>
<p>Web Sockets are new technology, part of HTML5 stack, for asynchronous communication between the web server and the browser. The client initiates a WebSocket connection by using the JavaScript APIs and from there on both the client and the server may send data to the other party fully asynchronously.</p>
<h2>Wicket Native WebSocket</h2>
<h3>Server side APIs</h3>
<p>Wicket Native WebSocket module provides an integration that makes it as easy to use WebSockets as you already use Ajax in your Wicket applications. Basically all you need to do is to add a WebSocketBehavior to a page/component and override the callback methods you may be interested in - onConnect, onClose and/or onMessage.</p>

{% highlight java %}
add(new WebSocketBehavior() {

  @Override protected void onMessage(WebSocketRequestHandler handler, TextMessage message) {
    String msg = message.getText();
    // use the message sent by the client

    handler.push("A message pushed by the server");
  }

});
{% endhighlight %}

<p>Most probably an application will need to push messages from the server to the client asynchronously, i.e. without the client to ask for such messages. This may be achived by storing the needed information to lookup the WebSocket connection when WebSocketBehavior#onConnect(ConnectedMessage) is called:</p>

{% highlight java %}
@Override protected void onConnect(ConnectedMessage message) {
	applicationName = message.getApplication().getName();
	sessionId = message.getSessionId();
	pageId = message.getPageId();

	// store applicationName, sessionId, pageId somehow so they are available later
}

{% endhighlight %}

<p>Later when you need to push a message asynchronously you may lookup the connection with:</p>

{% highlight java %}

  IWebSocketConnectionRegistry registry = new SimpleWebSocketConnectionRegistry();
  Application application = Application.get(applicationName);
  IWebSocketConnection wsConnection = registry.getConnection(application, sessionId, pageId);

  if (wsConnection != null && wsConnection.isOpen()) {
    wsConnection.send("Asynchronous message");
  }

{% endhighlight %}

<h3>Client side API</h3>
<p>By default when WebSocketBehavior is used Wicket will open a default WebSocket connection for you, so you can just use:</p>

{% highlight javascript %}
Wicket.WebSocket.send("A message sent by the client");
{% endhighlight %}

<p>to send a message from the client to the server.</p>
<p>To listen for messages pushed by the server you have to subscribe for a topic with name '/websocket/message':</p>

{% highlight javascript %}
Wicket.Event.subscribe("/websocket/message", function(jqEvent, message) {

	// do something with the message.
	// it may be a text or a binary message depending on what you pushed from the server side
});

{% endhighlight %}

<h2>Re-rendering components</h2>
<p>If you have noticed earlier #onMessage() receives a parameter of type <em title="org.apache.wicket.ajax.WebSocketRequestHandler">WebSocketRequestHandler</em>. This request handler implements AjaxRequestTarget so it is possible to add components (via #add(Component...)), to prepend/append JavaScript (via #prependJavaScript() and #appendJavaScript()) as you already do with Ajax behaviors.</p>

{% highlight java %}
@Override protected void onMessage(WebSocketRequestHandler handler, TextMessage message) {

	handler.add(someComponent);
}

{% endhighlight %}

<h2>Demo application</h2>
<p>At Martin Grigorov's GitHub <a href="https://github.com/martin-g/blogs/tree/master/wicket6-native-websockets">repository</a> you may find an  application that demonstrates some of the new possibilities available with WebSockets. The application schedules an asynchronous task at the server side that pushes data back to the client and visualizes it in Google Line chart.</p>
<h2>Documentation</h2>
<p>More complete documentation may be found at the <a href="https://cwiki.apache.org/WICKET/wicket-native-websockets.html">Wiki</a>. Feel free to contribute to make it better. Just create an account and edit it!</p>
