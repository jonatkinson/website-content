---
title: Setting up a DevPi server
slug: devpi-server
date: 2016-07-14 12:00:00+00:00
tags: code devpi python pypi
category:
link:
description:
type: text
---

Downloading 3rd party packages is an important part of our build and deployment process. While PyPI is generally excellent, it does present some risks for us:
 
 - PyPI does go down from time-to-time. While availability is so much better than it was a few years ago, downtime does still happen.
 - Older Python packages are occasionally removed from PyPI, leaving holes in our requirements.
 - Downloading packages to our servers requires external connectivity, and counts towards our bandwidth usage.

I was encouraged by <a href="https://nylas.com/blog/packaging-deploying-python">this article</a> (and the subsequent <a href="https://news.ycombinator.com/item?id=9861127">commentary</a>) to investigate using <a href="http://doc.devpi.net/latest/index.html">DevPI</a> as a proxy for PyPI:
 
> The MIT-licensed devpi system features a powerful PyPI-compatible server and a complimentary command line tool to drive packaging, testing and release activities with Python. 

> Main features and usage scenarios:

> fast PyPI mirror: use a local self-updating pypi.python.org caching mirror which works with pip and easy_install. After an initial cache-fill it can work off-line and will re-synchronize when you get online again.

This is a quick walk-through of setting up a DevPI server. We use <a href="http://uwsgi-docs.readthedocs.org/en/latest/index.html">uWSGI</a> and <a href="http://nginx.org/">nginx</a> to serve the application, and we made it available on a host which is available over the private interface of our internal network. It's assumed that the server has a working `supervisor` daemon running.

First, we need to do some basic setup. We create a folder for the application, and a data folder. Then build a virtualenv, and install the `devpi-server` package:

    $ mkdir /srv/www/devpi/ && mkdir /srv/www/devpi/data/ && cd /srv/www/devpi
    $ virtualenv env
    $ ./env/bin/pip install -q -U devpi-server
    
Check that it's installed correctly:

    $ ./env/bin/devpi-server --version
    2.2.2
    
Now, generate the configuration files:

    $ devpi-server --port 4040 --serverdir ./data/ --gen-config
    wrote gen-config/supervisor-devpi.conf
    wrote gen-config/nginx-devpi.conf
    wrote gen-config/crontab
    wrote gen-config/net.devpi.plist
    wrote gen-config/devpi.service
    
Link the `supervisor` config file into place (switch to a superuser for this):

    $ ln -s /srv/www/devpi/gen-config/supervisor-devpi.conf /etc/supervisor/conf.d/99-devpi.conf
    
Supervisor will need restarting to launch the new process:

    $ sudo service supervisor restart
    
You need to link the `nginx` configuration file into place (again, as a superuser), and then restart `nginx`. I edited the `hostname` directive in the nginx config so it would be addressable with my local DNS:

    $ ln -s /srv/www/devpi/gen-config/nginx-devpi.conf /etc/nginx/sites-enabled/99-devpi.conf
    $ sudo service nginx restart
    
Check the `nginx` configuration is working, and is responding (on your local machine):

    $ curl pypi.hostname.com
    {
      "type": "list:userconfig",
      "result": {
        "root": {
          "username": "root",
          "indexes": {
            "pypi": {
              "type": "mirror",
              "bases": [],
              "volatile": false
            }
          }
        }
      }
    }
    
Now, we need to setup the `root` user on this devpi instance. The root user's password is blank by default.

    $ devpi use http://pypi.hostname.com
    using server: http://pypi.hostname.com/ (not logged in)
    no current index: type 'devpi use -l' to discover indices
    ~/.pydistutils.cfg     : no config file exists
    ~/.pip/pip.conf        : no index server configured
    ~/.buildout/default.cfg: no config file exists
    always-set-cfg: no
    $ devpi login root --password ''
    logged in 'root', credentials valid for 10.00 hours
    $ devpi user -m root password=verysecurepassword
    user modified: root
    $ devpi logoff
    
You should setup a regular user account

    $ devpi user -c jonathan password=verysecurepassword email=jon@wearefarm.com
    
Next, login as that user, and tell that user to use the `root/pypi` repository as default. Any packages requested by this method will be automatically downloaded and cached by your devpi instance. This step will amend your `~/.pip/pip.conf` file to reflect this new index, and use it as default form now on.

    $ devpi login jonathan
    password for user jonathan:
    $ devpi use --set-cfg root/pypi
    
Lets just do a quick check to make sure the index is working, and `pip` is configured correctly:

    $ mkdir /tmp/devpi-test && cd /tmp/devpi-test
    $ virtualenv env
    New python executable in env/bin/python
    Installing setuptools, pip, wheel...done.
    $ source env/bin/activate
    $ pip install --trusted-host pypi.frm.io Django
    Collecting Django
    Downloading http://pypi.hostname.com/root/pypi/+f/a5d/397c65a880228/Django-1.8.3-py2.py3-none-any.whl (6.2MB)
    100% |████████████████████████████████| 6.2MB 2.7MB/s
    Installing collected packages: Django
    Successfully installed Django-1.8.3
        
Note that you need to use the `--trusted-host` flag to suppress the `pip` trust warning. Setting up this host over SSL is simple enough, and can be done in the `nginx` configuration file.        
            