---
title: Django coverage reports without unit tests
slug: django-coverage-reports-without-unit-tests
datetime: 2017-10-31 16:45:07+00:00
---

I was recently working on a Django project which had a lot of development effort spent over a wide range of features which never made it to launch. We wanted to analyse the codebase, in part to identify where we had over-delivered on features which didn't make the cut, but also to help the development team find and remove the now 'dead' code.

The first tool which sprang to mind was `coverage.py`, which can be used to evaluate unit test coverage. After a little digging, you can use the same coverage engine to detect which code is used while Django's `runserver` is active. The following examples assume you have a working Django and Python environment, and you're installing into a virtualenv:

	$ pip install coverage
	$ coverage run manage.py runserver --noreload

This will spawn a single-threaded server, which coverage can monitor. You can now exercise your code at will, either by manually testing the site or running some browser automation with TestCafe or Selenium. The quality of your coverage results will be influenced by how thorough you are at this stage.

Once you're finished, `^C` the coverage process, and it will create a `.coverage` file in the working directory. You can now generate and view a more useful HTML report with `coverage html && open htmlcov/index.html`. You'll notice this is very noisy; it includes all your dependencies in `env/`, and also various files which Django evaluates in full on startup. These skew your results, and make the document more difficult to read, so it's useful to exclude some results using `--omit`:

- Anything in the virtualenv folder (in my case `env/`).
- Any `__init__.py` files.
- All Django migrations.
- All Django `admin.py` files.
- All instances of `urls.py`.
	
The final command was as follows:

	$ coverage html --omit=*env*,apps/*/admin.py,apps/*/__init__.py,apps/*/migrations/*.py,apps/*/urls.py        
            