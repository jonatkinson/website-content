---
title: Listing ElasticBeanstalk applications with the aws CLI tool.
slug: listing-elasticbeanstalk-applications-with-aws-cli
datetime: 2018-01-16 12:11:22+00:00
---

You can easily list EB applications in your default AWS account with the following query.

	$ aws elasticbeanstalk describe-applications --query 'Applications[*].{Name:ApplicationName}' --output=text        
            
