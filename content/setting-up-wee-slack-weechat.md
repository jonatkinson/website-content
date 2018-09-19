---
title: Setting up wee-slack
slug: setting-up-wee-slack-weechat
datetime: 2018-08-09 07:38:28
---
### Installing the prerequisites

First, we need to install the base `weechat` binaries. I like the `aspell` plugin to be enabled in Weechat, so I'll install that first:

	$ brew install aspell
	$ aspell dicts # Check the available dictionaries.

Now, it's time to install `weechat` itself. Weechat has just gone through a transition between Python2 and Python3, and at the time of writing, installing from `brew` is affected by [this bug](https://github.com/Homebrew/homebrew-core/issues/30509), so we're going to explicitly install with Python2 support, and we will install the `websocket_client` library in the Python2 tree.

	$ brew install weechat --with-aspell --with-python@2
	$ sudo /usr/local/opt/python@2/bin/pip2 install websocket_client

### Installing `wee-slack`

Now it's time to install the `wee-slack` plugin. There are more secure ways to do this, but this will work for the purposes of this guide:

	$ mkdir -p ~/.weechat/python/autoload
	$ cd ~/.weechat/python/autoload
	$ wget https://raw.githubusercontent.com/wee-slack/wee-slack/master/wee_slack.py

Now, launch `weechat`, and you'll see the following message:

	ERROR: Failed connecting to Slack with token starting with INSERT
	VALID KE: invalid_auth
	ERROR: Token does not look like a valid Slack token. Ensure it is a valid token and not just a OAuth code.

To register, in the `weechat` console, type:

	/slack register

Follow the prompts onscreen. The redirect after authentication will fail, and this is expected, however you need to copy the `code` parameter from the redirect URL.

Return to `weechat`, and enter the following:

	/slack register [your code parameter here]
	/python reload slack

You should now be connected to your Slack instance. You can navigate using common IRC commands such as `/join` and `/query`. If you're setting up `weechat` for the first time, it will feel more comfortable to turn on mouse mode, which you can do like this:

	/set weechat.look.mouse on
	/mouse enable

In general, `wee-slack` works well. Certain integrations which post rich snippets to Slack (for example, the Bitbucket plugin) are quite visually noisy in Weechat, so I want t figure out how to filter or rewrite these to be more concise. But that's for another day.