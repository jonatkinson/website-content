---
title: create-react-native-app on High Sierra in 2018
slug: create-react-native-app-high-sierra
datetime: 2018-03-12 17:11:54+00:00
---

A really quick guide to getting started with `react-native` in 2018.

Let's see what we're dealing with:

	$ uname -a
	Darwin hostname.local 17.4.0 Darwin Kernel Version 17.4.0: Sun Dec 17 09:19:54 PST 2017; root:xnu-4570.41.2~1/RELEASE_X86_64 x86_64

If you don't speak `uname`, that means OSX High Sierra.

The '[Getting Started](http://facebook.github.io/react-native/docs/getting-started.html)' section of the official React Native documentation (at least at this point in time, the 0.54 release) recommends installing `create-react-native-app` with `npm`. This isn't going to fly with npm@5 due to various reasons, which you can find in this [total shit-show of a thread](https://github.com/npm/npm/issues/16991).

Unfortunately, I've already installed some `node`-related packages using `brew`. As such:

	$ npm --version
	5.6.0
	$ node --version
	v9.5.0

The workaround is to use `yarn`. Install it as follows:

	$ npm install -g yarn

Once this is complete, you can start a sample application:

	$ create-react-native-app test
	$ cd test
	$ yarn start

Inevitably, this will also break in seemingly ridiculous ways:

	$ yarn start
	yarn run v1.5.1
	$ react-native-scripts start
	16:51:43: Unable to start server
	See https://git.io/v5vcn for more information, either install watchman or run the following snippet:
  	  sudo sysctl -w kern.maxfiles=5242880
  	  sudo sysctl -w kern.maxfilesperproc=524288

	error An unexpected error occurred: "Command failed.

I'm faintly bemused. I suppose we need to tweak some kernel parameters?

	$ sudo sysctl -w kern.maxfiles=5242880
	Password:
	kern.maxfiles: 49152 -> 5242880
	$ sudo sysctl -w kern.maxfilesperproc=524288
	kern.maxfilesperproc: 24576 -> 524288

Now let's try again:

	$ yarn start
	yarn run v1.5.1
	$ react-native-scripts start
	16:58:17: Starting packager...
	Packager started!

	<.details omitted ...>

Okay, I'm quite tickled by the ANSI QR code. This can be opened with Expo on a mobile device.
