---
title: Recursively updating S3 bucket permissions
slug: recursively-updating-s3-bucket-permission
datetime: 2017-09-19 12:29:52+00:00
---

If you want to recursively apply a permission to an S3 bucket (for example, to add the `public-read` permission), then you can use the `aws` CLI tool to copy from a bucket to itself, and update the metadata as it does so. It's quicker that using the AWS console, anyway.

	$ aws s3 cp s3://bucketname/optional/path/ s3://bucketname/optional/path/ \
	--recursive \
	--metadata-directive REPLACE \
	--acl public-read \
	--cache-control max-age=31536000        
            
