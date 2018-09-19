---
title: http://1.2.3.8/bmi-int-js/bmi.js
slug: http1238bmi-int-jsbmijs
date: 2007-12-26 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I'm currently using my T-Mobile 3G connection to do most of my web browsing, what with it being Christmas, and that I've been travelling around a lot without reliable WiFi.

In contrast to a few years ago, when the hardware support for laptop to phone connections was terrible, that is now incredibly simple. I paired my phone and my laptop, then chose 'Connect to network' from the Bluetooth menu. My phone asked if this was okay, and everything worked pretty much flawlessly (and incidentally, I get a better upload speed over 3G than I do over my DSL at home).

So, I was happily doing some lazy Boxing day web development,and I started to notice errors in my Javascript. Not errors because I'd had a few beers, or errors because I was using a library incorrectly, but errors appearing in functions which I <em>hadn't even written</em>.

I checked the source of my pages, and noticed that someone or something was inserting Javascript into my pages. Specifically, a script from <a href="http://1.2.3.8/bmi-int-js/bmi.js">http://1.2.3.8/bmi-int-js/bmi.js</a>. The 'Oh shit, my server has been cracked' alarm went off pretty loudly, and after some peer-review, that changed to the 'Oh shit, someone has released a trojan for OSX for which I am very under prepared' alarm.

It turns out that T-Mobile (and Vodafone UK) think it is appropriate to insert their own Javascript into each page which I visit, which pipes all images through a proxy to degrade their quality. However, due to an improperly terminated newline, this script cannot be parsed by Firefox or Opera in conjunction with any XHTML 1.1 or XML documents.

This causes the sort of errors which <em>break all subsequent Javascript</em>. For sites with lots of Javascript, that is kind of a big deal. It isn't behaviour which I asked for, and clearly it hasn't been appropriately tested. Interestingly, script isn't even inserted when using Safari (come on, T-Mobile, browser sniffing is so very Nineties). In addition, it maps key events to Shift-R, which can cause problems with other web applications (for example, GMail), and access keys in Firefox.

This is a terrible solution to a problem which didn't really exist in the first place. I've paid for a data connection, so I expect the data I requested to be delivered to me <em>as requested</em>, not as T-Mobile think I should recieve it. If I download large images, then I'm happy to take responsibility for what I've done and pay the cost of those downloads. Very disappointing.
            
            