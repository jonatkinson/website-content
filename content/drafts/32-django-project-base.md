---
title: Django Project Base
slug: django-project-base
date: 2010-01-22 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

<em>"This is my project base. There are many like it, but this one is mine."</em>

Today I finally got around to putting my <a href="http://github.com/jonatkinson/project-base">Django project base</a> on <a href="http://github.com">Github</a>. I've been using this base for about six months now, and after a lot of rewrites and different approaches, it's now reasonably stable. I've been starting a lot of new projects recently, and repeatedly fixing the same small bugs in this project template, so I decided to spend a few hours this afternoon cleaning it up and making it public.

I'm not totally satisfied with my use of shell scripts to do some of the bootstrap actions (ideally I'd use Fabric for all of these tasks), and longer-term I want to make it easier to rename the django project rather than using search/replace in TextMate.

I know there are a lot of similar projects to this on Github, but none of them worked exactly like I wanted (I think too many depend on zc.buildout and similar), and while I'm not crazy about re-inventing the wheel, I hope there are enough other people out there who share my preferences who will find this useful.
            
            