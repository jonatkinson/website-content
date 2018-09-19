---
title: Fixing RubyGems out of memory errors
slug: fixing-rubygems-out-memory-errors
date: 2008-09-10 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I run Debian Etch on a 256MB Xen instance (provided by <a href="http://mampi.co.uk">Mampi</a>). For quite a long time I've been running into out of memory errors when using <a href="http://www.rubygems.org/">RubyGems</a>. The usual transcript would go something like this:

<pre>$ sudo gem install rails
Bulk updating Gem source index for: http://gems.rubyforge.org
Killed</pre>

For a while, I hacked around this problem by increasing my RAM to 512MB when I needed to run RubyGems (which is a terrible solution, but I have the ability to increase my RAM at will, and when all you have is a hammer, everything looks like a nail). I was using the <a href="http://packages.debian.org/etch/rubygems">packaged version of RubyGems</a> (0.90), which it turns out is horribly out of date now. I can't find a definitive source, but various mailing lists are pointing to an issue with the YAML parser causing Gems to use far too much memory.

The upgrade to a more recent version of RubyGems is fairly simple:

<pre>wget http://rubyforge.org/frs/download.php/38646/rubygems-1.2.0.tgz
tar xzf rubygems-1.2.0.tgz
cd rubygems-1.2.0.tgz
sudo ruby setup.rb</pre>

If you haven't removed your old (packaged) version of RubyGems, you will need to do so now, then create a symlink to the new Gems executable:

<pre>sudo ln -s /usr/bin/gem1.8 /usr/bin/gem</pre>

If you run the new version of Gems, you'll notice much reduced RAM usage.
            
            