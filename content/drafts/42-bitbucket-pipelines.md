---
title: Bitbucket Pipelines
slug: bitbucket-pipelines
date: 2016-06-01 12:00:00+00:00
tags: ci tools atlassian bitbucket pieplines
category:
link:
description:
type: text
---

We're heavy users of the Atlassian product suite at FARM, and that includes their powerful but obtuse CI system, <a href="https://www.atlassian.com/software/bamboo">Bamboo</a>. Our workflow consists of an automated pipeline from <a href="http://www.bitbucket.org">BitBucket</a>, to Bamboo running on a custom AWS EC2 image (which we have <a href="http://blog.wearefarm.com/2014/04/05/custom-bamboo-images/">blogged about before</a>), to our in-house dashboard system, called Wallboard.

While we're happy with this system, Bamboo at times has felt slightly cumbersome. It's hyper-configurable (sometimes at the expense of usability), and it carries obvious technical debt from it's days as an on-premise, "old IT" system which hasn't been well addressed by it's move to the cloud. Still, it was a pleasant surprise to hear about the launch of <a href="https://bitbucket.org/product/features/pipelines">BitBucket Pipelines</a>, which brings the CI process into BitBucket directly, and uses Docker rather than EC2. It feels like a more modern solution.

<img class="img-responsive" src="https://s3-eu-west-1.amazonaws.com/assets.jonatkinson.co.uk/live/static/images/pipelines-screenshot.png" alt="Screenshot of Bitucket Pipelines">

The configuration file is simple enough; for our purposes we just needed to specify the container we needed, and then run an existing `make tests` script from the repository (which handled dependencies and the build steps already):

	# You can use a Docker image from Docker Hub or your own container
	# registry for your build environment.
	image: python:2.7.11

	pipelines:
	  default:
		- step:
			script: # Modify the commands below to build your repository.
			  - make test

Obviously this is about as simple as this configuration file can get, though based on the <a href="https://confluence.atlassian.com/bitbucket/configure-bitbucket-pipelines-yml-792298910.html">documentation</a>, we could easily replicate the contents of our `Makefile` in the configuration YML.

Speed of execution is generally good. It appears that the build and test process kicked off as soon as the commit is complete, unlike Bamboo where sometimes we had to wait for an agent to come available, or for an EC2 instance to spin up. In general, our entire build pipeline (which is common across all our Django projects) of checkout, install dependencies, migrate database, and run tests was executing in around 3 minutes. This is considerably faster than with Bamboo, and could be further improved by, for example, removing the `pip wheel` compliation.

Merging CI/CD with the source control service makes sense. The pipelines run for each branch and pull request (and in parallel, which is helpful for busy repositories). Pipelines does everything it needs to do, and little more, which is common in most of the best tools we use. There are a few features which I'd like to see added, though as this is an early stage beta product I'm inclined to be quite forgiving:

- Parsing of test output. With Bamboo we liked the parsing of XUnit XML files to give specific insight into which tests failed. However, Pipelines gives direct visibility of the console output, so this may be something we can live without.
- Hipchat integration. I'm sure this is coming soon.

There is a lot to like about Pipelines. It's promising for organisations which are using the Atlassian suite of tools, and I respect their desire to launch this into beta at an early stage. The current 'lightweight' CI offerings are very focused on Github integration (eg. TravisCI), so having another comparable player in this space is an exciting development. We're looking forward to more.        
            