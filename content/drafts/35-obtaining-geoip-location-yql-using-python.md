---
title: Obtaining GeoIP location with YQL using Python
slug: obtaining-geoip-location-yql-using-python
date: 2010-04-20 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I've a few projects coming up for <a href="http://84labs.com">84labs</a> which required location awareness. Location awareness works great with any recent phone, but for traditional clients, I needed to fall-back to obtaining the location from the client's IP address.

There is an excellent free IP location database hosted on <a href="http://datatables.org/">datatables.org</a>, which offered the easiest way to get the data which I needed. This meant using <a href="http://developer.yahoo.com/yql/">YQL</a>, which I haven't used before; YQL is "an expressive SQL-like language that lets you query, filter, and join data across Web services".

So here is the code. I was using Python, Django and <a href="http://python-yql.org/">Python YQL module</a>, but the same query presumably works with any language you choose. I've removed a lot of exception handling for clarity.

	# Get the current user's IP address.
	client_ip_address = request.META['REMOTE_ADDR']

	# Create a YQL public query object.
	y = yql.Public()

	# Build the query.
	query = 'USE "http://www.datatables.org/iplocation/ip.location.xml" AS ip.location; select * from geo.places where woeid in (select place.woeid from flickr.places where (lat,lon) in(select Latitude,Longitude from ip.location where ip="%s"))' % client_ip_address;

	# Execute the query.
	result = y.execute(query)

	# .et voila.
	ip_place_name = result.rows['locality1']['content']
	ip_location = result.rows['centroid']</code>

That's it. The query just performs a simple select against the 'iplocation' database, then retrieves the latitude and longitude from the flickr.places database (flickr.places is part of the standard YQL set of databases, which is why we don't need a specific USE statement to be able to access it).
            
            