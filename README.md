Custom Django 1.6 Template
==========================

A personalized project template for Django 1.6 using postgres for development and production. Ready to deploy on Heroku.

Forked from the original [django-two-scoops-project](https://github.com/twoscoops/django-twoscoops-project).

Creating your Project
=====================

*Prerequisites: python, pip, django*

To create a new Django project, run the following command replacing `{{ project_name }}` with your actual project name:

    $ django-admin.py startproject --template=https://github.com/imkevinxu/django-twoscoops-project/archive/master.zip --extension=py,md,bat,html --name=Procfile,Makefile {{ project_name }}

Make virtual environments
-------------------------

*Prerequisites: virtualenv, virtualenvwrapper*

    $ cd {{ project_name }}
    $ mkvirtualenv {{ project_name }}-dev && add2virtualenv `pwd` && deactivate
    $ mkvirtualenv {{ project_name }}-prod && add2virtualenv `pwd` && deactivate

Install python packages
-----------------------

For development:

    $ deactivate && workon {{ project_name }}-dev
    $ pip install -r requirements/local.txt

For production:

    $ deactivate && workon {{ project_name }}-prod
    $ pip install -r requirements.txt

Development Mode
================

Create local postgres database for dev
--------------------------------------

*Prerequisites: Postgres*

Install Postgres for your OS [here](http://www.postgresql.org/download/). For Max OSX the easiest option is to download and run [Postgres.app](http://postgresapp.com/).

    $ createdb {{ project_name }}

Run project locally in dev environment
--------------------------------------

    $ deactivate && workon {{ project_name }}-dev
    $ python {{ project_name }}/manage.py runserver

Production Mode
===============

Set .env variable for prod
--------------------------

The environment variables for production must contain a separate SECRET_KEY for security and the appropriate DJANGO_SETTINGS_MODULE and PYTHONPATH in order to use django-admin.py seemlessly. Hacky use of `date | md5` to generate a pseudo-random string.

*.env is not version controlled so the first person to create this project needs to create a .env file for Foreman and Heroku to read into the environment. Future collaboraters need to email the creator for it.*

    $ echo "SECRET_KEY=`date | md5`" >> .env
    $ echo "DJANGO_SETTINGS_MODULE=config.settings.production" >> .env
    $ echo "PYTHONPATH={{ project_name }}" >> .env

Deploy to Heroku
----------------

*Prerequisites: heroku-config*

    $ git init
    $ git add .
    $ git commit -m "ready for heroku deploy"
    $ heroku create
    $ heroku config:push
    $ heroku config:pull
    $ git push heroku master
    $ heroku ps:scale web=1
    $ heroku open

Run project locally in prod environment
---------------------------------------

*Prerequisites: Heroku Toolbelt*

This is meant to mimic production as close as possible using both the production database and environment settings so proceed with caution.

    $ deactivate && workon {{ project_name }}-prod
    $ foreman run django-admin.py collectstatic --noinput
    $ foreman start

TODO
----

- Syncdb instructions
- Database migration instructions

Acknowledgements
================

![Two Scoops of Django](http://twoscoops.smugmug.com/Two-Scoops-Press-Media-Kit/i-C8s5jkn/0/O/favicon-152.png "Two Scoops Logo")

This project follows best practices as espoused in [Two Scoops of Django: Best Practices for Django 1.6](http://twoscoopspress.org/products/two-scoops-of-django-1-6).

Many thanks to:
---------------

- Daniel Greenfield and Audrey Roy for writing the book
- Randall Degges for django-skel
- All of the [contributors](https://github.com/twoscoops/django-twoscoops-project/blob/master/CONTRIBUTORS.txt) to the original fork
