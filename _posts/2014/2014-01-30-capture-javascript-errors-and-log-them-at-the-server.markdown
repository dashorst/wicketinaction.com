---
layout: post
status: publish
published: true
title: Capture JavaScript errors and log them at the server
author: martin-g
author_login: martin-g
author_email: martin.grigorov@gmail.com
wordpress_id: 1001
wordpress_url: http://wicketinaction.com/?p=1001
date: '2014-01-30 13:08:52 +0100'
date_gmt: '2014-01-30 11:08:52 +0100'
categories:
- Wicket
tags: []
comments: []
---

[ThoughtWorks technology radar][1] recommends capturing client-side
JavaScript errors and logging them at the server. This way you can see
whether your application experience problems on different browsers.

## A solution for Apache Wicket

Michael Haitz has implemented a [small
library][2]
that integrates with Wicket's JavaScript APIs and captures and sends
all client-side logs to the server.

Check its documentation to see how to setup and use it!

[1]: http://www.thoughtworks.com/radar/#/techniques/545 "ThoughtWorks technology radar"
[2]: https://github.com/l0rdn1kk0n/wicket-clientside-logging "Wicket client side logging"
