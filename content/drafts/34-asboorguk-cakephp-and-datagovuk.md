---
title: asbo.org.uk, CakePHP and data.gov.uk
slug: asboorguk-cakephp-and-datagovuk
date: 2010-01-25 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

Yesterday, I wrote <a href="http://asbo.org.uk">asbo.org.uk</a>, a site which provides really basic visualisation of the UK's anti-social behaviour order data from 1999-2007. This data was recently released by <a href="http://data.gov.uk">data.gov.uk</a>.

I wrote the whole site, wrangled the data, and deployed it yesterday afternoon, in about six hours. I've got some contract work coming up using CakePHP, so I wanted to try it out, and I wanted to see what I could do in such a short space of time. I'm quite pleased with the results. There's no analysis of the data, just presentation, but I was trying to see what I could do in the time I had, rather than develop features.

Originally, I wanted to write a 'how safe am I' sort of application, which could offer data about the types of crime most likely to occur in a given area, but this idea was pretty much killed by the time I saw the actual data available. Maybe I don't know enough about Excel-scraping, but I considerably reduced the scope of this project because the data was such a mess - it's just not worth the time to extract information from arbitrarily formatted spreadsheets. Formatting the single table of data used on <a href="http://asbo.org.uk">asbo.org.uk</a> took about three hours, which seems like a waste.

I was generally quite impressed with CakePHP; it's laid out in a sane way, though coming from Python some of the automatic discovery of models in the controllers feels a little bit magic, and I'm not sure I'm comfortable passing around arrays of data rather than data objects themselves, but it's not a deal breaker. I do like the layout system, something I commonly implement with blocks in Django, but seeing it formalised as part of the recommended approach to application templating is nice.

I put the <a href="http://github.com/jonatkinson/asbo">source on Github</a>,if anyone is interested.
            
            