---
layout: post
status: publish
published: true
title: Monitoring your app with a rabid rabbit
author: dashorst
author_login: dashorst
author_email: martijn.dashorst@gmail.com
author_url: http://martijndashorst.com
wordpress_id: 363
wordpress_url: http://wicketinaction.com/?p=363
date: '2009-03-27 13:24:36 +0100'
date_gmt: '2009-03-27 11:24:36 +0100'
categories:
- Events
- Wicket
tags:
- Meetup
- Wicket
- Amsterdam
- Rabbit
- Monitoring
comments:
- id: 447
  author: Jonathan Locke
  author_email: jonathan.locke@gmail.com
  author_url: ''
  date: '2009-03-27 16:58:23 +0100'
  date_gmt: '2009-03-27 14:58:23 +0100'
  content: That rabbit is all stick and no carrot, which is surprising. Shouldn't
    he say something nice when you resolve a bug or when the servers speed up again?
- id: 448
  author: dashorst
  author_email: martijn.dashorst@gmail.com
  author_url: http://martijndashorst.com
  date: '2009-03-27 17:19:22 +0100'
  date_gmt: '2009-03-27 15:19:22 +0100'
  content: Well, if the build is fixed, it announces it. But doing more would drive
    my co-workers crazy. We have to fix the nabaztag hudson plugin to support multiple
    rabbits, and be able to subscribe more than one rabbit to a project.
- id: 449
  author: Yann PETIT
  author_email: yann.petit@gmail.com
  author_url: http://www.normandyjug.org
  date: '2009-03-30 10:34:38 +0200'
  date_gmt: '2009-03-30 08:34:38 +0200'
  content: "You can find it in France in all SFR stores, and also in all FNAC stores.\r\nMay
    be I'm wrong but this is produced by a French company. \r\n\r\nI think it's very
    easy to find in Europe too, go to http://www.nabaztag.com/en/m-16-nabaztag-where-can-i-buy-a-nabaztag.html
    and search for a sell location.\r\n\r\nNot sure I'll be able to convince my boss
    to buy one, but I'll try ^^"
---
<p>In my talk for the <a href="http://wicketinaction.com/2009/03/wicket-meetup-amsterdam-2009-video-online/">Wicket Meetup in Amsterdam last tuesday</a>, I showed a <a href="http://www.amazon.com/gp/product/B000TM7B1A?ie=UTF8&tag=awidi-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=B000TM7B1A">WiFi Rabbit</a> called Nabaztag. This rabbit is used in our company to monitor our applications, build server and issue tracker.</p>
<p>[caption id="attachment_367" align="alignnone" width="423" caption="Nabaztag dressed up for Halloween as a vampire"]<img src="http://wicketinaction.com/wp-content/uploads/2009/03/keynotescreensnapz001.png" alt="Nabaztag dressed up for Halloween as a vampire" title="Nabaztag dressed up for Halloween" width="423" height="538" class="size-full wp-image-367" />[/caption]</p>
<p>The rabbit maintains a connection through Wifi with a bunch of servers in France. These servers provide all communication to your rabbit: text-to-speech, MP3, weather reports and internet radio. Letting your rabbit talk is as simple as sending a http request to the central servers. Violet has published the API"s where you can rotate the ears, change the led colors and perform text-to-speech in different voices and languages.</p>
<p>The text-to-speech is really nice, but at times the  sounds produced are surprising. Whenever a server goes down, our rabbit(s) start yelling. When a server is slow for our users, the rabbits start protesting. When issues are put in our bug tracker, the rabbit announces the issue, and title. When a build fails, the rabbit shouts in anger at us (but in a very polite, british accent with the voice of mrmuggles). There is a nabaztag plugin for Hudson making it easy to setup the rabbit and build notifications.</p>
<p>You can buy these rabbits for your company, and I promised to put a link to where you can shop for them. Probably at this moment, the Amazon shop is the cheapest option, but you have to have patience if you are in Europe.</p>
<ul>
<li><a href="http://www.amazon.com/gp/product/B000TM7B1A?ie=UTF8&tag=awidi-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=B000TM7B1A">Nabaztag @ Amazon.com</a></li>
<li><a href="http://www.amazon.co.uk/gp/product/B001MWRZOY?ie=UTF8&tag=martidasho-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=B001MWRZOY">Nabaztag @ Amazon.co.uk</a></li>
</ul>
