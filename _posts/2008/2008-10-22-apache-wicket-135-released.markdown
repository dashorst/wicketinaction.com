---
layout: post
status: publish
published: true
title: Apache Wicket 1.3.5 released!
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
excerpt: "<p>The Apache Wicket team is proud to announce the availability of the fifth
  maintenance release: Apache Wicket 1.3.5. A lot of bugs have been squashed and several
  improvements implemented. It is recommended you update to Wicket 1.3.5 at your earliest
  convenience.</p>\r\n\r\n<p>Eager people click here to download the distribution,
  others can read further:</p>\r\n\r\n<ul>\r\n\t<li><a href=\"http://www.apache.org/dyn/closer.cgi/wicket/1.3.5\">http://www.apache.org/dyn/closer.cgi/wicket/1.3.5</a></li>\r\n</ul>\r\n\r\n\r\n<p>We
  thank you for your patience and support.</p>\r\n\r\n- The Wicket Team\r\n\r\n"
wordpress_id: 242
wordpress_url: http://wicketinaction.com/?p=242
date: '2008-10-22 17:40:52 +0200'
date_gmt: '2008-10-22 15:40:52 +0200'
categories:
- Releases
- Wicket
tags:
- Wicket
- Releases
comments:
- id: 77
  author: Apache Wicket 1.3.5 is released! | A Wicket Diary
  author_email: ''
  author_url: http://martijndashorst.com/blog/2008/10/22/apache-wicket-135-is-released/
  date: '2008-10-22 17:46:09 +0200'
  date_gmt: '2008-10-22 15:46:09 +0200'
  content: '[...] Read more about this post [...]'
- id: 78
  author: Sortie de la version 1.3.5 de Wicket | NooCodeCommit
  author_email: ''
  author_url: http://www.noocodecommit.com/blog/nicogiard/wicket/sortie-de-la-version-135-de-wicket
  date: '2008-10-22 22:27:39 +0200'
  date_gmt: '2008-10-22 20:27:39 +0200'
  content: '[...] L&#8217;info officielle a été postée sur le blog de Wicket In Action
    [...]'
- id: 82
  author: ely
  author_email: ecelino@gmail.com
  author_url: ''
  date: '2008-10-24 05:55:40 +0200'
  date_gmt: '2008-10-24 03:55:40 +0200'
  content: congratulations wicket team.. i like wicket approach to web app development...
    (although i am having difficulties working with resources in wicket framework...)
    thank you for making such a great framework
---
<p>The Apache Wicket team is proud to announce the availability of the fifth maintenance release: Apache Wicket 1.3.5. A lot of bugs have been squashed and several improvements implemented. It is recommended you update to Wicket 1.3.5 at your earliest convenience.</p>
<p>Eager people click here to download the distribution, others can read further:</p>
<ul>
<li><a href="http://www.apache.org/dyn/closer.cgi/wicket/1.3.5">http://www.apache.org/dyn/closer.cgi/wicket/1.3.5</a></li>
</ul>
<p>We thank you for your patience and support.</p>
<p>- The Wicket Team</p>
<p><a id="more"></a><a id="more-242"></a></p>
<h2>Apache Wicket</h2>
<p>Apache Wicket is a component oriented Java web application framework. With proper mark-up/logic separation, a POJO data model, and a refreshing lack of XML, Apache Wicket makes developing web-apps simple and enjoyable again. Swap the boilerplate, complex debugging and brittle code for powerful, reusable components written with plain Java and HTML.</p>
<p>You can find out more about Apache Wicket on our website:</p>
<ul>
<li><a href="http://wicket.apache.org">http://wicket.apache.org</a></li>
</ul>
<h2>This release</h2>
<p>This release is the fifth maintenance release for the Wicket 1.3 product. This release fixes several bugs and adds some minor improvements. You can find out about the changes at the bottom of this announcement.</p>
<h3>Migrating from 1.2</h3>
<p>If you are coming from Wicket 1.2, you really want to read our migration guide, found on the wiki:</p>
<ul>
<li><a href="http://cwiki.apache.org/WICKET/migrate-13.html">http://cwiki.apache.org/WICKET/migrate-13.html</a></li>
</ul>
<h3>Downloading the release</h3>
<p>You can download the release from the official Apache mirror system, and you can find it through the following link:</p>
<ul>
<li><a href="http://www.apache.org/dyn/closer.cgi/wicket/1.3.5/">http://www.apache.org/dyn/closer.cgi/wicket/1.3.5/</a></li>
</ul>
<p>For the Maven and Ivy fans out there: update your pom&#039;s to the following, and everything will be downloaded automatically:</p>
{% highlight xml %}
<dependency>
  <groupId>org.apache.wicket</groupId>
  <artifactId>wicket</artifactId>
  <version>1.3.5</version>
</dependency>
{% endhighlight %}
<p>Substitute the artifact ID with the projects of your liking to get the other projects.</p>
<p>Please note that we don&#039;t prescribe a Logging implementation for SLF4J. You need to specify yourself which one you prefer. Read more about SLF4J here: <a href="http://slf4j.org">http://slf4j.org</a></p>
<h3>Validating the release</h3>
<p>The release has been signed by Martijn Dashorst, your release manager for today. The public key can be found in the KEYS file in the download area. Download the KEYS file only from the Apache website.</p>
<ul>
<li><a href="http://www.apache.org/dist/wicket/1.3.5/KEYS">http://www.apache.org/dist/wicket/1.3.5/KEYS</a></li>
</ul>
<p>Instructions on how to validate the release can be found <a href="http://www.apache.org/dev/release-signing.html#check-integrity">here</a>.</p>
<h3>Reporting bugs</h3>
<p>In case you do encounter a bug, we would appreciate a report in our JIRA:</p>
<ul>
<li><a href="http://issues.apache.org/jira/browse/WICKET">http://issues.apache.org/jira/browse/WICKET</a></li>
</ul>
<h3>The distribution</h3>
<p>In the distribution you will find a README. The README contains instructions on how to build from source yourself. You also find a CHANEGELOG-1.3 which contains a list of all things that have been fixed, added and/or removed since Wicket 1.3.0.</p>
<h2><a name="News-ReleaseNotesWicketVersion1.3.5"></a>Release Notes - Wicket - Version 1.3.5</h2>
<h3>Sub-task</h3>
<ul>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1805">WICKET-1805</a> - Allow to change charset in StringRequestTarget: change CharSet used by the OutStream as well</li>
</ul>
<h3><a name="News-Bug"></a>Bug</h3>
<ul>
<li><a href="https://issues.apache.org/jira/browse/WICKET-406">WICKET-406</a> - form fields are reset when a file upload fails</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-431">WICKET-431</a> - Modal window can not be closed after session timeout</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-622">WICKET-622</a> - Component.toString() is unsafe</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-625">WICKET-625</a> - Wicket doesn&#039;t clean up properly when hot-deploying; hangs onto Class references.</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-847">WICKET-847</a> - setResponsePage redirects to wrong url</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-861">WICKET-861</a> - NumberFormatException with UrlCompressingWebRequestProcessor in WicketTester</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-928">WICKET-928</a> - Exception when clicking two times rapidly on the "next" button in a wizard</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1003">WICKET-1003</a> - Modal Window Does Not Close When Using IndicatingAjaxButton</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1104">WICKET-1104</a> - Modal window sticks to cursor on resize</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1120">WICKET-1120</a> - Problem closing a ModalWindow when used through an IFrame</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1161">WICKET-1161</a> - DiskPageStore should write the sessions index file to disk on destroy (from WicketFilter.destroy())</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1205">WICKET-1205</a> - Relative path calculations for inline paths in non-bookmarkable pages are incorrect on Tomcat.</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1346">WICKET-1346</a> - Generated HTML page contains incorrect content when "WicketMessage: No get method defined for class"</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1376">WICKET-1376</a> - Using AbstractAjaxTimerBehavior and mounting that page gives exception</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1413">WICKET-1413</a> - RequestUtils.toAbsolutePath() has a bug regarding to UTF-8 encoded page parameter.</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1425">WICKET-1425</a> - appendToInit() method is not called in class DatePicker</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1437">WICKET-1437</a> - Pagemap lock calculates timeout wrong</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1449">WICKET-1449</a> - &#039;./&#039; appended to URL causes HTTP 404 in Internet Explorer (using root context)</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1450">WICKET-1450</a> - Ajax Re-render does not work after AbstractRestartResponseException()</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1471">WICKET-1471</a> - FeedbackPanel does not work properly in clustered environment</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1478">WICKET-1478</a> - AbortWithWebErrorCodeException in onBeforeRender causes WicketRuntimeException</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1494">WICKET-1494</a> - IntegerConvert cannot handle locale specific input/output</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1496">WICKET-1496</a> - DataTable.html does not validate (HTML 4.01/XHTML-Strict)</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1520">WICKET-1520</a> - JavaScriptStripper breaks prototype.js</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1535">WICKET-1535</a> - ExternalLink JavaScript not working in FF 3</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1577">WICKET-1577</a> - Int based PropertyModel throws convertion error</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1582">WICKET-1582</a> - WicketTester executeAjaxEvent onclick generating non-AJAX response</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1634">WICKET-1634</a> - ClassName needs conversion from Path to dotted notation in AutoLinkResolver</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1636">WICKET-1636</a> - WebPage.onAfterRender compares configurationType String using ==  operator, not equals method</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1648">WICKET-1648</a> - AbstractRequestTargetUrlCodingStrategy(line 174) throws confusing exception. It would be better redirect to 404-page in this case.</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1652">WICKET-1652</a> - Hard-coded quotes in xml prologue</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1664">WICKET-1664</a> - WebPage ERROR message</li>
<div style="display: none"><a href='http://spyontextmessagesms.com/' title='how to spy on text messages'>how to spy on text messages</a></div>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1679">WICKET-1679</a> - Memory leak in DiskPageStore</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1704">WICKET-1704</a> - ResourceStreamRequestTarget.configure set wrong ContentLength for non-ascii characters</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1713">WICKET-1713</a> - ModalWindow JavaScript does not restore tabIndexes correctly on IE 6</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1714">WICKET-1714</a> - PackagedTextTemplate does not load resource from application resource stream locator</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1719">WICKET-1719</a> - StringResourceModel may fail to format numbers using MessageFormat</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1724">WICKET-1724</a> - Clicking on AjaxLink (when used on a page mounted through QueryStringUrlCodingStrategy) after session-expiry throws a NullPointerException in IE and Safari (i.e. in BookmarkableListenerInterfaceRequestTarget.processEvents)</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1728">WICKET-1728</a> - remove obsolete check from LocalizedImageResource</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1730">WICKET-1730</a> - RfcCompliantEmailAddressValidator accepts whitespace and tab</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1731">WICKET-1731</a> - When used in inherited markup, <wicket:link> tries to load a class with an illegal name</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1736">WICKET-1736</a> - Allow Access to AutoCompleteTextField AutoCompleteBehavior</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1737">WICKET-1737</a> - wicketTester does not find HTML mark-up if custom location is used.</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1740">WICKET-1740</a> - RequestCycle.urlFor modifies page parameters</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1746">WICKET-1746</a> - gecko: ajax javascript reference rendering problem</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1755">WICKET-1755</a> - In html Include component isAbsolute method returns false for an absolute path in unix-like systems</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1759">WICKET-1759</a> - Typo in method name: AttributeModifier#replaceAttibuteValue</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1765">WICKET-1765</a> - Extending from org.apache.wicket.Page causes StackOverflowError</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1769">WICKET-1769</a> - AjaxPagingNavigation usare results in AjaxPaginNavigator not found exception</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1773">WICKET-1773</a> - DiskPageStore-FileNotFoundException</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1777">WICKET-1777</a> - Overflow when setting Expires header in WebResource</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1780">WICKET-1780</a> - NPE in feedback panel</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1787">WICKET-1787</a> - AjaxSubmitLink in Internet Explorer does not work with Wicket&#039;s automatically genreated id&#039;s</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1788">WICKET-1788</a> - "Invalid procedure call or argument" on AJAX call with IE7</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1789">WICKET-1789</a> - Border fails to render if its contents are not visible by default</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1793">WICKET-1793</a> - Problem with submit</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1797">WICKET-1797</a> - Bug with default RadioChoice "for" attribute on label generation.</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1799">WICKET-1799</a> - wicket-extensions has unused reference to commons-collections.jar</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1806">WICKET-1806</a> - JavascriptStripper ignores context when looking for multiline comments</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1816">WICKET-1816</a> - Wicket 1.3.4 violates servlet standard, Glassfish spews warnings</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1818">WICKET-1818</a> - wicket:id attribute with a value containing spaces generates invalid markup</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1820">WICKET-1820</a> - Embedded forms do not support multipart</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1827">WICKET-1827</a> - AutoCompleteTextField shows completion list even if focus is not in the text field anymore</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1829">WICKET-1829</a> - MarkupComponentBorder skips first tag in MarkupStream</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1834">WICKET-1834</a> - Invalid Cookie Names for persistence used according to RFC (doesn&#039;t work in tomcat 6.x)</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1839">WICKET-1839</a> - IAjaxIndicatorAware/WicketAjaxIndicatorAppender with AutoCompleteTextField doesn&#039;t work</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1843">WICKET-1843</a> - Disabling RadioGroup via authorization strategy does not disable contained Radio buttons</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1846">WICKET-1846</a> - Dutch text message for NumberValidator incorrect</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1857">WICKET-1857</a> - Unfound markup information is not entirely cached even in deployment mode</li>
</ul>
<h3>Improvement</h3>
<ul>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1502">WICKET-1502</a> - Slow opening of ModalWindow for complex pages</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1622">WICKET-1622</a> - expose the IItemFactory in RefreshingView</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1638">WICKET-1638</a> - Wickets unique IDs cause problems for automated testing</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1692">WICKET-1692</a> - on Java 6+ DatePicker.localize should use DateFormatSymbols.getInstance(Locale) instead of new DateFormatSymbols(Locale)  to support DateFormatSymbolsProviders</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1748">WICKET-1748</a> - 304 Last Modified responses should include an Expires header</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1770">WICKET-1770</a> - PagingNavigation&#039;s javadoc contains malformed html snippet</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1782">WICKET-1782</a> - Protection against CSRF (cross-site request forgery) attacks</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1795">WICKET-1795</a> - Make it possible for to encode unicode strings in component</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1801">WICKET-1801</a> - Make AbstractDefaultAjaxBehavior.findIndicatorId() protected</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1802">WICKET-1802</a> - Propertyresolver could be more informative</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1810">WICKET-1810</a> - StringRequestTarget is bloated and needs some care</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1830">WICKET-1830</a> - Include Component Path in Generated Markup</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1840">WICKET-1840</a> - Defaults to ReloadingWicketFilter in maven&#039;s archetype and quickstart</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1844">WICKET-1844</a> - Wizard button implementations should not be final</li>
</ul>
<h3>New Feature</h3>
<ul>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1716">WICKET-1716</a> - make autocompleter more customizable</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1720">WICKET-1720</a> - Add clearLocalizerCache to Application JMX bean</li>
</ul>
<h3>Wish</h3>
<ul>
<li><a href="https://issues.apache.org/jira/browse/WICKET-399">WICKET-399</a> - Make RestartResponseAtInterceptPageException with a SignIn-type page work correctly from AjaxFallbackLink</li>
<li><a href="https://issues.apache.org/jira/browse/WICKET-1758">WICKET-1758</a> - Make DiskPageStore#getSessionFolder protected (rather than private)</li>
</ul>
<div style="display: none">zp8497586rq</div>
