---
title: SSH on multiple ports with OSX 10.5
slug: ssh-multiple-ports-osx-105
date: 2008-12-01 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I want my Mac Mini running Leopard to listen for SSH connections on multiple ports. This proved to be far more difficult that it should have been. For the sake of this example, lets say SSH should listen on port 22, which is the default, and port 10022.

On most unixes, you just edit /etc/sshd_config to contain the following:

<pre>Port 22
Port 10022</pre>

Then you restart SSH, and you're done. Things aren't so simple since Apple introduced launchd.

First, you need to duplicate the existing launchd service description file, like this:

<pre>sudo cp /System/Library/LaunchDaemons/ssh.plist /System/Library/LaunchDaemons/ssh2.plist</pre>

Then, edit the ss2.plist file as follows. The keys actually changed are Label and SockServiceName:

<pre>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;!DOCTYPE plist PUBLIC &quot;-//Apple//DTD PLIST 1.0//EN&quot; &quot;http://www.apple.com/DTDs/PropertyList
-1.0.dtd&quot;&gt;
&lt;plist version=&quot;1.0&quot;&gt;
&lt;dict&gt;
        &lt;key&gt;Label&lt;/key&gt;
        &lt;string&gt;com.openssh.sshd2&lt;/string&gt;
        &lt;key&gt;Program&lt;/key&gt;
        &lt;string&gt;/usr/libexec/sshd-keygen-wrapper&lt;/string&gt;
        &lt;key&gt;ProgramArguments&lt;/key&gt;
        &lt;array&gt;
                &lt;string&gt;/usr/sbin/sshd&lt;/string&gt;
                &lt;string&gt;-i&lt;/string&gt;
        &lt;/array&gt;
        &lt;key&gt;SHAuthorizationRight&lt;/key&gt;
        &lt;string&gt;system.preferences&lt;/string&gt;
        &lt;key&gt;SessionCreate&lt;/key&gt;
        &lt;true/&gt;
        &lt;key&gt;Sockets&lt;/key&gt;
        &lt;dict&gt;
                &lt;key&gt;Listeners&lt;/key&gt;
                &lt;dict&gt;
                        &lt;key&gt;Bonjour&lt;/key&gt;
                        &lt;array&gt;
                                &lt;string&gt;ssh&lt;/string&gt;
                                &lt;string&gt;sftp-ssh&lt;/string&gt;
                        &lt;/array&gt;
                        &lt;key&gt;SockServiceName&lt;/key&gt;
                        &lt;string&gt;ssh2&lt;/string&gt;
                &lt;/dict&gt;
        &lt;/dict&gt;
        &lt;key&gt;StandardErrorPath&lt;/key&gt;
        &lt;string&gt;/dev/null&lt;/string&gt;
        &lt;key&gt;inetdCompatibility&lt;/key&gt;
        &lt;dict&gt;
                &lt;key&gt;Wait&lt;/key&gt;
                &lt;false/&gt;
        &lt;/dict&gt;
&lt;/dict&gt;
&lt;/plist&gt;</pre>

Finally, you need to add your new SSH port to /etc/services. Append lines like this at the end of that file:

<pre>ssh2              10022/udp 
ssh2              10022/tcp</pre>

Now you need to instruct launchd to start this service (it should start automatically on bootup thereafter)

<pre>sudo launchctl load -w /System/Library/LaunchDaemons/ssh2.plist</pre>

Finally, check that everything works correctly:

<pre>ssh -p 10022 localhost</pre>
            
            