---
layout: post
status: publish
published: true
title: Apache Wicket 1.3.6 released!
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
excerpt: "<p>The Apache Wicket team is proud to announce the availability of the sixth
  maintenance release: Apache Wicket 1.3.6. A lot of bugs have been squashed and several
  improvements implemented. It is recommended you update to Wicket 1.3.6 at your earliest
  convenience.</p>\r\n<p>Eager people click here to download the distribution, others
  can read further:</p>\r\n<ul><li><a href=\"http://www.apache.org/dyn/closer.cgi/wicket/1.3.6\">http://www.apache.org/dyn/closer.cgi/wicket/1.3.6</a></li></ul>\r\n<p>We
  thank you for your patience and support.</p>\r\n<p>- The Wicket Team</p>\r\n"
wordpress_id: 378
wordpress_url: http://wicketinaction.com/?p=378
date: '2009-05-04 17:01:09 +0200'
date_gmt: '2009-05-04 15:01:09 +0200'
categories:
- Releases
- Wicket
tags:
- Wicket
- Releases
comments: []
---
<p>The Apache Wicket team is proud to announce the availability of the sixth maintenance release: Apache Wicket 1.3.6. A lot of bugs have been squashed and several improvements implemented. It is recommended you update to Wicket 1.3.6 at your earliest convenience.</p>
<p>Eager people click here to download the distribution, others can read further:</p>
<ul>
<li><a href="http://www.apache.org/dyn/closer.cgi/wicket/1.3.6">http://www.apache.org/dyn/closer.cgi/wicket/1.3.6</a></li>
</ul>
<p>We thank you for your patience and support.</p>
<p>- The Wicket Team</p>
<p><a id="more"></a><a id="more-378"></a></p>
<h3>Apache Wicket</h3>
<p>Apache Wicket is a component oriented Java web application framework. With proper mark-up/logic separation, a POJO data model, and a refreshing lack of XML, Apache Wicket makes developing web-apps simple and enjoyable again. Swap the boilerplate, complex debugging and brittle code for powerful, reusable components written with plain Java and HTML.</p>
<p>You can find out more about Apache Wicket on our website:</p>
<ul>
<li><a href="http://wicket.apache.org">http://wicket.apache.org</a></li>
</ul>
<h3>This release</h3>
<p>This release is the fifth maintenance release for the Wicket 1.3 product. This release fixes several bugs and adds some minor improvements. You can find out about the changes at the bottom of this announcement.</p>
<h4>Migrating from 1.2</h4>
<p>If you are coming from Wicket 1.2, you really want to read our migration guide, found on the wiki:</p>
<ul>
<li><a href="http://cwiki.apache.org/WICKET/migrate-13.html">http://cwiki.apache.org/WICKET/migrate-13.html</a></li>
</ul>
<h4>Downloading the release</h4>
<p>You can download the release from the official Apache mirror system, and you can find it through the following link:</p>
<ul>
<li><a href="http://www.apache.org/dyn/closer.cgi/wicket/1.3.6/">http://www.apache.org/dyn/closer.cgi/wicket/1.3.6/</a></li>
</ul>
<p>For the Maven and Ivy fans out there: update your pom's to the following, and everything will be downloaded automatically:</p>
<p>&lt;dependency&gt;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&lt;groupId&gt;org.apache.wicket&lt;/groupId&gt;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&lt;artifactId&gt;wicket&lt;/artifactId&gt;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&lt;version&gt;1.3.6&lt;/version&gt;<br />
&lt;/dependency&gt;</p>
<p>Substitute the artifact ID with the projects of your liking to get the other projects.</p>
<p>Please note that we don't prescribe a Logging implementation for SLF4J. You need to specify yourself which one you prefer. Read more about SLF4J here: http://slf4j.org</p>
<h4>Validating the release</h4>
<p>The release has been signed by Igor Vaynberg, your release manager for today. The public key can be found in the KEYS file in the download area. Download the KEYS file only from the Apache website.</p>
<ul>
<li><a href="http://www.apache.org/dist/wicket/1.3.6/KEYS">http://www.apache.org/dist/wicket/1.3.6/KEYS</a></li>
</ul>
<p>Instructions on how to validate the release can be found here:</p>
<ul>
<li><a href="http://www.apache.org/dev/release-signing.html#check-integrity">http://www.apache.org/dev/release-signing.html#check-integrity</a></li>
</ul>
<h4>Reporting bugs</h4>
<p>In case you do encounter a bug, we would appreciate a report in our JIRA:</p>
<ul>
<li><a href="http://issues.apache.org/jira/browse/WICKET">http://issues.apache.org/jira/browse/WICKET</a></li>
</ul>
<h4>The distribution</h4>
<p>In the distribution you will find a README. The README contains instructions on how to build from source yourself. You also find a CHANEGELOG-1.3 which contains a list of all things that have been fixed, added and/or removed since Wicket 1.3.0.</p>
<h3>Release Notes - Wicket - Version 1.3.6</h3>
<h4>Bug</h4>
<ul>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-693">WICKET-693</a>] - What to do with the wicket dtd?</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1403">WICKET-1403</a>] - Reinjection fails after Server restart</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1483">WICKET-1483</a>] - Unusual ClassCastException (SimpleAttributeModifier to IBehaviorListener) processing onError.</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1504">WICKET-1504</a>] - AutoCompleteTextField - javascript error &quot;type mismatch&quot; in line 227 in IE</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1567">WICKET-1567</a>] - AutoLinkResolver does not work with &lt;a href=&quot;#internal&quot;&gt;Internal link&lt;/a&gt;</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1605">WICKET-1605</a>] - onclick is null or not an object in IE6, IE7; Form.appendDefaultButtonField</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1619">WICKET-1619</a>] - PagingNavigator.setEnabled(false) doesn&#x27;t work</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1689">WICKET-1689</a>] - style resources not looked up correctly in markup inheritance</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1781">WICKET-1781</a>] - ParentResourceEscapePathTest fails on OS X using cmd line maven</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1855">WICKET-1855</a>] - Using an AjaxSubmitLink outside of a Form does not set the form property</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1864">WICKET-1864</a>] - MockHttpServletRequest does not support absolute redirection URLs.</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1868">WICKET-1868</a>] - i18n package resource resolving depends too much on available locale</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1871">WICKET-1871</a>] - org.apache.wicket.util.string.Strings#stripJSessionId StringIndexOutOfBoundsException</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1886">WICKET-1886</a>] - WicketTester Cookie handling</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1890">WICKET-1890</a>] - Palette.onBeforeRender() throws IllegalArgumentException in cases when Palette is invisible.</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1901">WICKET-1901</a>] - Spelling error in fonts list in CaptchaImageResource</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1903">WICKET-1903</a>] - RadioChoice disable certain choice bug</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1904">WICKET-1904</a>] - CheckBox incorrectly converts its model value when a custom Boolean converter is installed - again</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1906">WICKET-1906</a>] - AutocompleteTextField throws javascript error Object Required</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1909">WICKET-1909</a>] - Wrong translation for StringValidator.range in Application_pl.properties</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1914">WICKET-1914</a>] - Form.appendDefaultButtonField produces invalid JavaScript if Ajax is disabled</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1915">WICKET-1915</a>] - wicket:message sometimes broken</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1930">WICKET-1930</a>] - FileUpload.writeToTempFile uses field Id as filename - Windows doesn&#x27;t support some characters</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1943">WICKET-1943</a>] - Wicket is changing the HTML within an input tag, autocomplete attribute from off to false</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1955">WICKET-1955</a>] - Error about misplaced &lt;wicket:head&gt; very uninformative and incorrect</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1959">WICKET-1959</a>] - PropertyResolver causes memory leaks with proxies</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1970">WICKET-1970</a>] - package goal does not work , com.thoughtworks.xstream.converters.enums.EnumMapConverter issue</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1998">WICKET-1998</a>] - setResponsePage redirects to wrong url</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2006">WICKET-2006</a>] - The page set by setReponsePage does not process its own response.</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2037">WICKET-2037</a>] - Should adding AJAX behaviour to a page make it stateful?</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2038">WICKET-2038</a>] - Missing redirects in AjaxPagingNavigationLink and AjaxPagingNavigationIncrementLink</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2048">WICKET-2048</a>] - HtmlProblemFinder documentation bug</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2077">WICKET-2077</a>] - SerializationChecker issue</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2079">WICKET-2079</a>] - Component Use Check always fails for visible components inside an invisible border body</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2087">WICKET-2087</a>] - typo in SpringBeanLocator.java</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2091">WICKET-2091</a>] - Error feedback is hidden by lower level messages</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2095">WICKET-2095</a>] - error in modal.js wrong use of typeof</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2096">WICKET-2096</a>] - MultiFileUploadField.js can&#x27;t find file input when serving pages as XHTML</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2100">WICKET-2100</a>] - DynamicImageResouce blocks loading of AjaxLazyLoadPanel</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2109">WICKET-2109</a>] - IResourceStream.close is not called by ResourceStreamRequestTarget</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2130">WICKET-2130</a>] - Pages stored in Session.touchedPages aren&#x27;t detached when part of ModalWindow</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2131">WICKET-2131</a>] - RequestCycle.urlFor does not escape &amp; properly</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2140">WICKET-2140</a>] - FormComponentPanel should not add a name attribute</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2142">WICKET-2142</a>] - Getting live sessions from RequestLogger results in NPE</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2151">WICKET-2151</a>] - WicketSessionFilter doesn&#x27;t takes into account WebApplication#getSessionAttributePrefix(WebRequest)</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2154">WICKET-2154</a>] - ServletWebRequest#getURL does not return relative URLs</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2156">WICKET-2156</a>] - StringResourceModel&#x27;s Localizer cannot be overwritten</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2191">WICKET-2191</a>] - WebApplication is not thread-safe</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2234">WICKET-2234</a>] - typo in pom.xml</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2236">WICKET-2236</a>] - Palette problem in IE7 Problem</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2243">WICKET-2243</a>] - WicketSessionFilter assumes that the WicketFilter has already been inited</li>
</ul>
<h4>Improvement</h4>
<ul>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1895">WICKET-1895</a>] - AjaxButton should have a constructor to set the label</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1921">WICKET-1921</a>] - Add an extension of AutoCompleteTextField which includes default css</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1965">WICKET-1965</a>] - Remove final from MarkupCache#clear()</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1984">WICKET-1984</a>] - MarkupContainer&#x27;s add(final Component child) does not initially check for a child null reference</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1986">WICKET-1986</a>] - MarkupContainer&#x27;s addOrReplace(final Component child) does not initially check for a child null reference</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2072">WICKET-2072</a>] - Allow for maps in the widgetProperties</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2090">WICKET-2090</a>] - Need reliable hook for storing/restoring data to/from page metadata that is tes compatbile</li>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-2160">WICKET-2160</a>] - application_nl.properties is outdated</li>
</ul>
<h4>New Feature</h4>
<ul>
<li>[<a href="http://issues.apache.org/jira/browse/WICKET-1900">WICKET-1900</a>] - Implement isEscapeLabalMarkup for RadioChoice</li>
</ul>
