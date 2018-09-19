---
title: Removing the toolbar in MacVim
slug: removing-toolbar-macvim
date: 2008-10-13 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I'm sure this is nothing new to anyone, but I only just discovered how to hide the toolbar in MacVim (and the associated guioptions preferences). In .vimrc:

<pre>if has("gui_running")
    set guioptions=egmrt
endif</pre>
            
            