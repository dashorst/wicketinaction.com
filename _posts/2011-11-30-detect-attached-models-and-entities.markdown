---
layout: post
status: publish
published: true
title: Detect attached models and entities in your component hierarchy
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 707
wordpress_url: http://wicketinaction.com/?p=707
date: '2011-11-30 15:33:00 +0100'
date_gmt: '2011-11-30 13:33:00 +0100'
categories:
- Wicket
tags:
- hibernate
- Best practices
- JPA
comments: []
---
<p><a href="http://stackoverflow.com/questions/8323261/extending-wickets-serialization-test">Adrian Cox asked a great question</a> over at StackOverflow on how to prevent (Hibernate) entities from being attached to your Wicket component tree, thus generating all those nasty exceptions like StaleObjectException, LazyInitException and the like:</p>
<blockquote><p>JPA managed objects must not be stored in the session. Instead, JPA managed objects are loaded on each request through detachable models.</p></blockquote>
<p>From  <a href="http://stackoverflow.com/questions/8323261/extending-wickets-serialization-test">Extending Wickets serialization test - Stack Overflow</a>.</p>
<p>I have previously shown at several Wicket meetups that this is feasible. See slide 23 and onward on <a href="http://www.slideshare.net/dashorst/keep-your-wicket-application-in-production">this slideshare presentation</a> (the rest of the presentation is also worth your while).</p>
<p>At Topicus we use the same kind of check for our applications as well. Being bitten by entities as fields, or final references, non-detached models or suddenly re-attached models we came to the conclusion that we needed to check for these errors. The <code>EntityAndSerializerChecker</code> was the result of our coding efforts.</p>
<p>Basically what you do is copy Wicket's serializer checker code and modify it to include your check as well as checking for serialization errors.</p>
<p>The check we created checks for our common base class, and wether or not the entity has been persisted. If so, we flag the error. Here's the meat of our checker (the check method rewritten from Wicket's serializer check):</p>

{% highlight java %}
private void check(Object obj) {
    if (obj == null || obj.getClass().isAnnotationPresent(Deprecated.class)
        || obj.getClass().isAnnotationPresent(SkipClass.class)) {
        return;
    }

    Class< ? > cls = obj.getClass();
    nameStack.add(simpleName);
    traceStack.add(new TraceSlot(obj, fieldDescription));

    if (!(obj instanceof Serializable) && (!Proxy.isProxyClass(cls))) {
        throw new WicketNotSerializableException(toPrettyPrintedStack(obj.getClass().getName())
            .toString(), exception);
    }
    if (obj instanceof IdObject) {
        Serializable id = ((IdObject) obj).getIdAsSerializable();
        if (id != null && !(id instanceof Long && ((Long) id) <= 0)) {
            throw new WicketContainsEntityException(toPrettyPrintedStack(
                obj.getClass().getName()).toString(), exception);
        }
    }
    if (obj instanceof LoadableDetachableModel) {
        LoadableDetachableModel< ? > ldm = (LoadableDetachableModel< ? >) obj;
        if (ldm.isAttached()) {
            throw new WicketContainsAttachedLDMException(toPrettyPrintedStack(
                obj.getClass().getName()).toString(), exception);
        }
    }
}
{% endhighlight %}

<p>For Wicket 1.5 we created our own <code>PageStoreManager</code> that performs these checks (and a lot of other things, like enabling a server side browsing history for our users). We provided our own RequestAdapter by overriding <code>PageStoreManager#newRequestAdapter(IPageManagerContext context)</code> and doing the serialization check in the adapter:</p>

{% highlight java %}
class DetachCheckingRequestAdapter extends RequestAdapter {
    public DetachCheckingRequestAdapter(IPageManagerContext context) {
        super(context);
    }

    @Override
    protected void storeTouchedPages(List<IManageablePage> touchedPages) {
        super.storeTouchedPages(touchedPages);
        if (Application.get().usesDevelopmentConfig()) {
            for (IManageablePage curPage : touchedPages) {
                if (!((Page) curPage).isErrorPage())
                    testDetachedObjects(curPage);
            }
        }
    }

    private void testDetachedObjects(final IManageablePage page) {
        try {
            NotSerializableException exception = new NotSerializableException();
            EntityAndSerializableChecker checker = new EntityAndSerializableChecker(exception);
            checker.writeObject(page);
        }
        catch (Exception ex) {
            log.error("Couldn't test/serialize the Page: " + page + ", error: " + ex);
            Session.get().setDetachException(ex);
        }
    }
}
{% endhighlight %}

<p>Additionally we have an Ajax callback in our base page that checks the session attribute to see if there was a serialization error. If so, we redirect to the error page with the serialization failure, to ensure that developers don't ignore the entity in page hierarchy. You can't show an error directly because when the check is performed, Wicket has already sent the markup to the browser and closed the connection. </p>
<p>So there you go: checking for serialization errors and persisted entities in your component hierarchy. Works like a charm and will drive your developers nuts at first, but will save a lot of money chasing down production bugs.</p>
