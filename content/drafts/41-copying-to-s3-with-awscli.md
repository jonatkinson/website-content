---
title: "Notebook: Copying to S3 with awscli"
slug: copying-to-s3-with-awscli
date: 2016-12-13 12:22:55+00:00
tags: s3 aws notebook cli
category:
link:
description:
type: text
---

Copying to S3 with `awscli` is essentially the same syntax as SCP. This assumes you have a correctly configures `awscli` (if not, run `aws configure` beforehand). First, simulate the transfer:

    $ aws s3 cp . s3://my-bucket-name --recursive --dryrun

Finally, remove `--dryrun` to start the copy process.        
            
