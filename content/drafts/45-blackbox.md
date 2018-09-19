---
title: Blackbox - Automated Website Quality Checks
slug: blackbox
date: 2014-12-05 12:00:00+00:00
tags: blackbox test oss python
category:
link:
description:
type: text
---

Like many agencies, at <a href="http://www.wearefarm.com">FARM</a> we host and maintain a broad estate of web sites and applications. 

For all these sites, you can usually mix and match any of the following criteria:

- We wrote the code.
- We didn't write the code.
- We wrote the code some time in the last 5 years.
- We get paid to maintain the code.
- We don't get paid to maintain the code.

These, combined with the two fundamental truths of the internet (1. the internet is a hostile place and, 2. best practices are always changing), mean that it's a constant battle to keep the sites we run patched, healthy, and responding quickly.

Doing this manually is becoming close to impossible, so for some time we have been using <a href="https://bitbucket.org/wearefarm/blackbox/">`blackbox`</a>, a collection of black-box tests which we use to run automated checks against our sites. 

We currently run these tests in our CI environment (which is powered by <a href="https://www.atlassian.com/software/bamboo">Atlassian Bamboo</a>), as this allows us to easily schedule testing, and provides a nice front-end to the test results. In the studio, we get very easily digestible green/red overview of which sites are meeting our quality standards, and which need attention. It's also possible to run these tests straight from the desktop, to test sites as they are in development.

Today, we're open-sourcing <a href="https://bitbucket.org/wearefarm/blackbox/">`blackbox`</a>, because it's the kind of tool which all agencies can benefit from. This first release automates the testing of:

- Canonical redirects (from no subdomain to `www.`).
- Testing a `robots.txt` file is present.
- Testing the `robots.txt` file isn't blocking too much content.
- Test a favicon is in place.
- Test that none of the on-page resources are returning a 404 or 500 error (for example, scripts or images)
- Test a sitemap.xml is included.
- Test that gzip responses are enabled.
- Test that the homepage of the site returns a 200 response.
- Test that a randomly generated URL returns a proper 404 response.

We also added a few <a href="http://www.djangoproject.com">Django</a>-specific tests, which are:

- Test that settings.DEBUG is False.
- Test that the `django.contrib.sites.Sitemap` object is correct.

Our internal branch of `blackbox` also does some automated security testing, but we want to review this internally a little longer before we open-source that, too.

<a class="embedly-card" href="https://bitbucket.org/wearefarm/blackbox/?1">wearefarm / blackbox</a>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>        
            