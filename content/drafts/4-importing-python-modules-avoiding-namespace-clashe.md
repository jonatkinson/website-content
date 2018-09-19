---
title: Importing python modules, avoiding namespace clashes
slug: importing-python-modules-avoiding-namespace-clashe
date: 2007-10-31 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

Python does dumb things when importing modules.

The scenario; one local source file called 'calendar.py', and one Python module (part of the standard libraries), 'calendar'. If, within calendar.py, you evaluate

<pre>>>> import calendar</pre>

then the script will import itself. This is both proper, and useful, but there is no way to force the interpreter to import the module. This is quite annoying. In fact, <a href="http://docs.python.org/tut/node8.html#SECTION008110000000000000000">the documentation</a> identifies this issue (emphasis mine):

"â€¦it is important that the script not have the same name as a standard module, or Python will attempt to load the script as a module when that module is imported. This will generally be an error."

The problem here (beyond the manual acknowledging this is a common error, and nothing being done to remedy it), is that Python is encouraging non-standard and obscure naming of packages. I'm writing source which deals with a calendar, so in the interests of my own sanity (and the sanity of my successor), I'm not going to name my file 'joncalendar.py', 'calendar2.py', or anything like that. I should be able to name my files as I see correct.

So, here is the workaround. By removing the current directory from the Python path, prior to importing a module, you can import from the standard Python library.

<pre>def python_import(name):
    import os
    import sys
    current_path = os.path.dirname(os.path.abspath(__file__))
    base_name = os.path.basename(current_path).split('.')[0]
    sys.path[:] = [path for path in sys.path
        if os.path.abspath(path) != os.path.abspath(current_path)]
    original_module = sys.modules[name]
    del sys.modules[name]
    python_module = __import__(name)
    python_module_name = 'python_%s' % name
    sys.modules[python_module_name] = python_module
    sys.path.append(current_path)
    sys.modules[name] = original_module
    return python_module</pre>

An example of usage:

<pre>calendar = python_import('calendar')
print dir(calendar)</pre>
            
            