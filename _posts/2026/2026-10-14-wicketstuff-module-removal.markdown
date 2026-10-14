---
layout: post
status: publish
published: true
title: Removal of 14 obsolete wicketstuff core modules
author: dashorst
date: '2026-10-14 10:00:00 +0200'
date_gmt: '2026-10-14 08:00:00 +0000'
categories:
- Wicket
tags: [wicketstuff]
comments: []
---
We have removed 14 old modules from wicketstuff core. Most of them integrate third-party libraries or services that are abandoned or no longer exist. Some have not had an update in over a decade (YUI, for example). Starting with releases 10.12.0 and 11.0.0, the wicketstuff project no longer publishes these modules.

Every module takes time to maintain, build and release. We would rather spend that time elsewhere.

## Do you still depend on one of these modules?

You have two options:

1. **Switch to a supported alternative.** Each removed module has its own ticket, linked from [issue #1592](https://github.com/wicketstuff/core/issues/1592), and each ticket suggests an alternative.
2. **Copy the code into your project** and maintain the Wicket side yourself. [Commit 7f9a3c8](https://github.com/wicketstuff/core/tree/7f9a3c8b904540e076511ef53ee49cb5275d1f0d) is the last one that still contains the modules.

## The removed modules

| Module | Why it is obsolete |
| --- | --- |
| `yui-parent` | YUI 2.9.0 calendar |
| `wicketstuff-datetime-yui-parent` | YUI 2 date picker, Joda-Time |
| `wicket-datetime-parent` | Joda-Time; `java.time` support in wicket-extensions replaces it |
| `googlecharts-parent` | Google shut down the Image Charts API in 2019 |
| `flot-parent` | Bundles Flot 0.5 and excanvas |
| `jqplot-parent` | jqPlot has had no maintenance since 2016 |
| `simile-timeline-parent` | MIT SIMILE Timeline is abandoned |
| `openlayers-parent` | OpenLayers 2; use `openlayers3-parent` |
| `htmlcompressor-parent` | htmlcompressor 1.5.x is abandoned |
| `gae-initializer-parent` | You can no longer deploy to the App Engine Java 8 runtime |
| `wicketstuff-glassfish4-integration` | GlassFish 3.1/4.0 CDI proxy serializer |
| `wicket-bundle-parent` | The Wicket jars are OSGi bundles themselves |
| `wicket-facebook-parent` | Facebook discontinued its social plugins |
| `nashorn-parent` | The sandbox needs the SecurityManager, which is disabled since JDK 24 |

## Thank you

I want to thank the original authors of these modules, and everyone who maintained them over the years, for their work. It was time to send these modules off to spend their days on the pastures of a really nice farm in the countryside.

Martijn Dashorst, for the wicketstuff team
