---
title: Joining PDFs with Ghostscript
slug: joining-pdfs-ghostscript
date: 2008-11-11 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

If you need to join together a large amount of PDF files, but don't particularly want to pay for Acrobat Pro, <a href="http://pages.cs.wisc.edu/~ghost/">Ghostscript</a> can do the same.

First, download Ghostscript, then build it in the usual manner:

<pre>tar xjf ghostscript-xxx.tar.bz2
cd ghostscript-xxx
./configure --prefix=/usr/local
make
sudo make install</pre>

Then you can join multiple PDF files like this:

<pre>gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=output.pdf input1.pdf input2.pdf</pre>
            
            