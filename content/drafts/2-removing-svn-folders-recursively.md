
---
title: Removing .svn folders recursively
slug: removing-svn-folders-recursively
date: 2007-09-06 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

If you want to recursively delete all the Subversion folders from a working copy (for deployment or whatnot), this works well:

<pre>rm -rf `find . -type d -name .svn`</pre>
            
            