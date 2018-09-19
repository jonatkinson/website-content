---
title: Django's user_passes_test and generic views
slug: djangos-user_passes_test-and-generic-views
date: 2008-09-26 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

Previously, I've often used a combination of django.contrib.auth and the  login_required decorator as a simple way of controlling access to certain parts of an application. However, I'm working on a fairly complex application right now, which will grant access based on the is_staff and is_superuser fields.

It's trivial (<a href="http://docs.djangoproject.com/en/dev/topics/auth/?from=olddocs#limiting-access-to-logged-in-users-that-pass-a-test">and well documented</a>) to use the user_passes_test decorator in a view, but in my case I'm using generic views. This is not so well documented. Here is a basic example of how to do it:

<pre>from django.conf.urls.defaults import *
from django.contrib.auth.decorators import user_passes_test

staff_required = user_passes_test(lambda u: u.is_staff)
superuser_required = user_passes_test(lambda u: u.is_superuser)

urls = patterns('',
    (r'^staff_only/$', staff_required(generic.view.here), { .}),
    (r'^super_only/$', superuser_required(generic.view.here), { .}),
)</pre>
            
            