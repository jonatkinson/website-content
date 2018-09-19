---
title: Creating Xen DomU's, now with actual connectivity
slug: creating-xen-domus-now-actual-connectivity
date: 2007-11-02 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

<a href="http://xen-tools.org/software/xen-tools/">xen-tools</a> is a useful connection of scripts.

However, when you're creating a new Xen guest, don't forget to specify the correct gateway and netmask. Otherwise, you may end up spending a <em>whole evening</em> trying to figure out a routing problem which doesn't actually exist.

<pre>xen-create-image --hostname=example.com\ 
                      --ip=XXX.XXX.XXX.XXX\
                      --gateway=<strong>XXX.XXX.XXX.XXX</strong>\
                      --netmask <strong>255.255.255.0</strong>\
                      --dist=etch\
                      --passwd\
                      --boot</pre>
            
            