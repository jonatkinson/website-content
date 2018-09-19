---
title: Using the hash key in Emacs with a UK keyboard
slug: using-hash-key-emacs-uk-keyboard
date: 2008-11-26 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I've been using Emacs recently, and it took me a while to figure out how to make option-3 produce a hash symbol like other Cocoa applications.

This assumes you're using mac-key-mode like this:

<pre>(add-to-list 'load-path "~/.emacs.d/mac-key-mode/")
(require 'mac-key-mode)
(mac-key-mode 1)
(setq mac-option-modifier 'control)</pre>

To rebind the hash key, just drop the following in your .emacs:

<pre>(global-unset-key (kbd "C-3"))
(global-set-key (kbd "C-3") '(lambda() (interactive) (insert-string "#")))</pre>

            
            