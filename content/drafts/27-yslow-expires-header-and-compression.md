---
title: YSlow, expires header and compression
slug: yslow-expires-header-and-compression
date: 2009-09-21 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

Every time I need to raise a pitiful <a href="http://developer.yahoo.com/yslow/">YSlow</a> score, I follow the same recipe, and every time I do so, I realise I've not written it down anywhere. In my continuing quest to turn convert this blog from interesting narrative to boring snippets archive, I present the appropriate apache.conf stanza:<pre># Enable Expires header
ExpiresActive On
ExpiresByType image/gif "access plus 30 days"
ExpiresByType image/jpeg "access plus 30 days"
ExpiresByType image/png "access plus 30 days"
ExpiresByType text/css "access plus 30 days"
ExpiresByType application/x-javascript "access plus 30 days"
ExpiresByType image/x-icon "access plus 30 days"
ExpiresByType text/html "access plus 1 hour"
<br /># Enable gzip compression
AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css application/x-javascript</pre>

Enabling the required modules is left as an exercise to the reader, on a Debian-ish system, the modules are installed as 'deflate' and 'expires'.
            
            