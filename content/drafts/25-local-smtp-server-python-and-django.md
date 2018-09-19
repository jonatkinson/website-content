---
title: Local SMTP server with Python and Django
slug: local-smtp-server-python-and-django
date: 2009-09-21 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

Like most things, I'm probably the last to know about this, but it's very useful.

When I'm locally developing Django applications which use SMTP, I usually get stuck in a cycle which goes: make request, send, wait, check inbox, wait, wait, there-it-is, oh, it's wrong, I've wasted 3 minutes. Annoying.

This snippet runs a local SMTP server, which echoes all incoming mail to stdout, instantly:

<pre><code>sudo /usr/lib/python2.5/smtpd.py -n -c DngServer localhost:25</code></pre>

Note that sudo is only required to run it on the standard low port, but you can specify a different port number in settings.py if you don't have privileges.
            
            