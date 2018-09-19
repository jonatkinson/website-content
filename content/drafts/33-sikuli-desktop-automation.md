---
title: Sikuli desktop automation
slug: sikuli-desktop-automation
date: 2010-01-24 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

There's quite a lot of <a href="http://twitter.com/#search?q=sikuli">buzz around</a> <a href="http://sikuli.csail.mit.edu/index.shtml">Project Sikuli</a> at the moment, so I spent time today playing with it.

Sikuli is a GUI automation engine which uses a vision engine to identify elements on screen. In practise, it works well as a quick way to script repetitive desktop actions,  without having to learn the AppleScript actions an application provides, or how to hook into a desktop accessibility framework to manipulate applications. Instead, you tell the Sikuli engine how you expect regions of the screen to look, and then how to further manipulate them with clicks, key-presses and so on.

That is quite an abstract explanation, so here is a practical example.

Every morning, when I sit down at my desk, I do two things: I do some basic maintenance on my Mac, and I print out my day planner sheet. This only takes maybe 5 minutes, but I need to wait in front of the screen and click the right buttons at the right time. That's pretty dull, so here's how I'm using Sikuli to automate that.

First of all, the maintenance script. I use <a href="http://www.macpaw.com/cleanmymac">CleanMyMac</a> to perform one big system clean every morning, which requires a few steps; launching the application, scanning for files to clean, approving the list of files, running the cleaning process, then closing the application.

My Sikuli script for doing this looks like this:

<img src="http://jonatkinson.co.uk/static/files/sikuli_clean_script.png" />

The script is fairly self-explanatory. First, it switches to (or opens) the CleanMyMac application, then runs the scan process. The script sleeps for five seconds, the searches for the 'scan finished' message, and goes to sleep again until that message is displayed. Then it performs the clean operation, and again waits for the 'clean finished' message. After this, it closes the application.

At no point is this script passing events directly to the application, nor is it querying the application to get information about its state; it's just examining the frame-buffer and looking for patterns which are similar to those specified in the script. On the Sikuli website, they claim their vision engine is smart enough to still work even if an application slightly changes it's visual style, but I haven't been able to test this.

This is the script for printing my day planner:

<img src="http://jonatkinson.co.uk/static/files/sikuli_pages_script.png" />

First, it closes then re-opens Pages so that it is in a predictable state. Then, the script manipulates the 'recent documents' drop-down, to open my 'Day Sheet 2' document. Notice that the script uses the 'wait' function frequently so that the vision engine isn't searching for an image before it has been drawn by the operating system. Once the document is open, we pass the operating system the &#8984;P keyboard shortcut to open the print dialogue, then click 'print'. Finally, Pages is closed, and I check my printer tray. Here's a screen-cast of this in action. Note that I use the 'Run and show each action' button to start the script. This way, you can see the vision engine matching and highlighting each element on the screen:

<object width="425" height="344"><param name="movie" value="http://www.youtube.com/v/HvIEKz6qQO0&hl=en_US&fs=1&hd=1"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/HvIEKz6qQO0&hl=en_US&fs=1&hd=1" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="344"></embed></object>

Sikuli does have some limitations; You can't copy and paste between scripts, which I think is due to how the IDE stores image region data on the filesystem. Also, scripts seem wedded to the IDE, so you can't launch a script without needing to click the 'run' button in the IDE, but as the IDE is really just a thin wrapper over an underlying Jython instance, I'm sure this would be possible with a little more digging.

The obvious next step is to properly test how well Sikuli does deal with visual changes; using Sikuli to test web applications would be a great addition to the toolbox (and it would eliminate the problems with brittle tools like <a href="http://seleniumhq.org/">Selenium</a>), and the IDE and language is simple enough for non-programmers to take some of the burden of writing tests.

I'm quite looking forward to the future of Sikuli. I'd like to see this visual search technology make it into more traditional scripting environments like AppleScript, though I'm not sure that'll happen any time soon, but anything which reduces reliance on bending traditional accessibility frameworks to perform in this role is a step forward.
            
            