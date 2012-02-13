Heroku buildpack: PyPy
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpack) for Python apps.
It uses [virtualenv](http://www.virtualenv.org/) and [pip](http://www.pip-installer.org/).

Usage
-----

To create a new Heroku Cedar application with this buildpack:

    $ heroku create --stack cedar --buildpack git://github.com/mtigas/heroku-buildpack-pypy.git

To update an existing Heroku Cedar application to use this buildpack:

    $ heroku config:add BUILDPACK_URL=git://github.com/mtigas/heroku-buildpack-pypy.git

and then re-deploy your application.

----


Example:

    $ ls
    Procfile  requirements.txt  web.py

    $ heroku create --stack cedar --buildpack git://github.com/mtigas/heroku-buildpack-pypy.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom build pack... done
    -----> Python app detected
    -----> Downloading pypy-1.8
    -----> Preparing virtualenv version 1.7
           Already using interpreter /tmp/buildpack_3nkm61jrytp4x/pypy-1.8/bin/pypy
           New pypy executable in ./bin/pypy
           Installing distribute............done.
           Installing pip...............done.
    -----> Activating virtualenv
    -----> Installing dependencies using pip version 1.0.2
           Downloading/unpacking Flask==0.7.2 (from -r requirements.txt (line 1))
           Downloading/unpacking Werkzeug>=0.6.1 (from Flask==0.7.2->-r requirements.txt (line 1))
           Downloading/unpacking Jinja2>=2.4 (from Flask==0.7.2->-r requirements.txt (line 1))
           Installing collected packages: Flask, Werkzeug, Jinja2
           Successfully installed Flask Werkzeug Jinja2
           Cleaning up...

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root. It will detect your app as Python/Django if there is an additional `settings.py` in a project subdirectory.

It will use virtualenv and pip to install your dependencies, vendoring a copy of the Python runtime into your slug.  The `bin/`, `include/` and `lib/` directories will be cached between builds to allow for faster pip install time.

Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

To change the vendored virtualenv, unpack the desired version to the `src/` folder, and update the virtualenv() function in `bin/compile` to prepend the virtualenv module directory to the path. The virtualenv release vendors its own versions of pip and setuptools.
