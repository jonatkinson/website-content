---
title: FileMerge keyboard shortcuts
slug: filemerge-keyboard-shortcuts
date: 2008-09-18 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I really like Apple's <a href="http://en.wikipedia.org/wiki/Apple_Developer_Tools#FileMerge">FileMerge</a>. I like it because it is simple, and I like it because the interface is apparently unchanged since NeXTSTEP. It feels like using a well-worn hand tool of some kind.

I don't like the fact that it requires use of the mouse for all but the most simple of tasks (the only merge-window shortcuts are the left and right cursor keys, for 'merge left' and 'merge right'), and there the help pages don't list any of the keyboard shortcuts. When you have a thousand line file to merge, and some of the changes require 'choose both' or 'choose neither', using the mouse becomes rather tiresome rather quickly.

Fortunately, you can edit FileMerge's interface definition file quite easily to add some extra keyboard shortcuts, or modify the existing ones. Provided below is my .nib file for FileMerge, which defines the following shortcuts:<ul><li><strong>Left</strong> - Choose left</li><li><strong>Right</strong> - Choose right</li><li><strong>Ctrl-Left</strong> - Choose both, left first</li><li><strong>Ctrl-Right</strong> - Choose both, right first</li><li><strong>Space</strong> - Choose Neither</li></ul>

You can <a href="http://jonatkinson.co.uk/static/files/diffview.nib.zip">download my interface definition</a>, unzip, then drop the diffview.nib file into<pre>/Developer/Applications/Utilities/FileMerge/Contents/Resources/English.lproj/</pre>

Restart FileMerge and you'll have working keyboard shortcuts. Much faster!
            
            