---
title: Headless Virtualbox on OSX
slug: headless-virtualbox-osx
date: 2010-01-03 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

When I'm developing, I try to continuously deploy to a realistic environment as often as possible. This means a Debian server running a stack as close to production as I can get. Of course, I don't deploy to the actual production servers (a lesson I've learnt many times over), so I virtualise a Debian box and <a href="http://www.debianadmin.com/clone-your-ubuntu-installation.html">clone package set</a> from the production server. Combined with bridged networking and a quick hosts file change, and I've got a production-equivalent server always available at server.local.

Hence, I use <a href="http://www.virtualbox.org/">VirtualBox</a> <em>all</em> the time on my Mac, and I've got at least one server instance running all the time. But the Virtualbox.app clutters up the dock, and I really just want the instance available via SSH, I don't care about actually seeing its framebuffer.

Fortunately, you can run headless Virtualbox instances on OSX. It just takes a little extra work. Assuming you installed Virtualbox in the default location, headless Virtualbox binaries live in:<pre>$ ls /Applications/VirtualBox.app/Contents/MacOS | grep "VBoxHeadless"
VBoxHeadless
VBoxHeadless-amd64
VBoxHeadless-x86
VBoxHeadless.dylib</pre><p>You can easily run an instance in headless mode by specifying the unique name of the VM on the command line (replace "Debian Server" with the name of your VM):<pre>/Applications/Virtualbox.app/Contents/MacOS/VBoxHeadless --startvm "Debian Server"</pre><p>Of course, it's a pain to have to start these instances each time you boot the host system, but you can have the VM start on login by adding a LoginHook. Here's how:<pre>sudo defaults write com.apple.loginwindow LoginHook /Applications/Virtualbox.app/Contents/MacOS/VBoxHeadless --startvm "Debian Server"</pre><p>If you want to stop the VM, just use whatever ACPI shutdown method the guest instance provides (for a Debian server, sudo /sbin/init 0 will suffice), and the VBoxHeadless process will silently quit. If you're virtualising a graphical OS, like Windows, you should probably check the documentation to use with VBoxHeadless; you can easily specify a VRDP address so you can connect to the instance via any RDP client.
            
            