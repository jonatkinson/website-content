---
title: Installing M2Crypto on OSX
slug: installing-m2crypto-osx
date: 2008-10-09 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I really like <a href="http://chandlerproject.org/Projects/MeTooCrypto">M2Crypto</a>, but it is difficult to install on OSX without a few annoying prerequisites. Assuming you are running Leopard (or, I guess, newer), this is what you need to do:

First, you need to install the <a href="http://developer.apple.com/tools/xcode/">XCode Developer Tools</a>. You probably have these installed already, but if not, they're on your OS install discs, or you can download them from Apple. Once they're installed, you should have a working C compiler. Test with:

<pre>$ gcc
i686-apple-darwin9-gcc-4.0.1: no input files</pre>

Now, you need to install the OpenSSL headers. Grab the latest OpenSSL package from <a href="http://www.openssl.org/source/">here</a>. Now, unpack and build it. You might want to put the kettle on, this takes a little while.

<pre>$ tar xzf openssl-x.x.x.tar.gz
$ cd openssl-x-x-x
$ ./config --prefix=/usr/local
$ make && make test
$ sudo make install</pre>

Next, <a href="http://www.swig.org/download.html">download SWIG</a>. Again, this needs unpacking and building:

<pre>$ tar xzf swig-x.x.x.tar.gz
$ cd swig-x.x.x
$ ./configure --prefix=/usr/local
$ make
$ sudo make install</pre>

Finally, you need to download and install M2Crypto. <a href="http://chandlerproject.org/Projects/MeTooCrypto">Grab the source</a>, and install it as follows:

<pre>$ tar xzf M2Crypto-x.x
$ cd M2Crypto-x.x
$ python setup.py build build_ext --openssl=/usr/local
$ sudo python setup.py install build_ext --openssl=/usr/local</pre>

Then to test your installation, simply do the following:

<pre>$ python
Python 2.5.1 (r251:54863, Jan 17 2008, 19:35:17) 
[GCC 4.0.1 (Apple Inc. build 5465)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import M2Crypto
>>></pre>
            
            
