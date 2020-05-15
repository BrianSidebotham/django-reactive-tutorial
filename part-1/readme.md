# Django Reactive Application Part 1

Getting started in Django is really quite straight forward. You don't need a lot to get started. I'm running Fedora 32 and so my standard python is python3.8 - if I refer to the python binary, I mean python3 if you must distinguish between versions. Nothing here is python 2.x.

>**NOTE:** By default Django uses SQLite as a data backend. In this tutorial, the reactive component has a requirement of postgres for the backend and so this tutorial will use postgres as the backend for django

## Installing Django

In this tutorial we will work in a [python 3 virtual environment](https://docs.python.org/3/library/venv.html) so you don't have to do anything as root. You won't muddy your system and you can remove everything once you're done.

First, we'll create a python virtual environment (python3 remember!) within this repositories directory:

```sh
    git clone BLAH/django-reactive-tutorial
    cd django-reactive-tutorial
    mkdir env && cd env
    python -m venv .
```

Everytime you want to work within this environment and do thngs for the tutorial, you'll have to be in the virtual environment. We enter the virtual environment by opening a terminal and sourcing the `bin/activate` script. The prompt will change to show the virtual environment you're currently in.

```sh
[bjs@localhost env]$ source bin/activate
(env) [bjs@localhost env]$
```

We can now go ahead and install the django framework:

```sh
(env) [bjs@localhost env]$ pip install django
```

We've picked up version `3.0.6` for this tutorial. Major and/or minor version changes from that might render this tutorial inaccurate for you.

We can go straight ahead and start a new django project now (Still within the python virtual environment):

```sh
django-admin startproject tutorial
```

This creates a sub-directory which contains various django files which make up the entire project.

Have a look around if you're not familiar with django and see what's there.

## Install Postgres

>**NOTE:** If you already have a postgres server available then you can skip this section.

As I noted at the start of the tutorial, we're interested in using a postgres backend for our project because one of the components we're going to use requires it. So before we go ahead and run the project, let's install postgres and configure django to use it. If you've already got a postgres server installed, you can go ahead and skip this step.

We do however need the `pg_config` binary so that a supporting postgres python library will work. For Fedora we can find out what provides that:

```sh
[bjs@localhost tutorial]$ dnf whatprovides pg_config

libpq-devel-12.2-1.fc32.x86_64 : Development files for building PostgreSQL client tools
Repo        : fedora
Matched from:
Filename    : /usr/bin/pg_config
```

I also want to have a postgres server running locally - you can choose to do this with something like docker, or else you can just go ahead and run a postgres server locally.

You can do a super-quick postgres install on fedora with:

```
    sudo dnf install -y postgresql-server postgresql-contrib pgadmin3
    sudo systemctl enable postgresql
    sudo firewall-cmd --permanent --add-port=5432/tcp
    sudo postgresql-setup --initdb --unit postgresql
    sudo systemctl start postgresql
```

Which should get you a local postgres server running. I'll be using another development postgres server for this tutorial. You can also use a docker container instead of course.

## Create Postgres 

## Install Postgres Python Support

Within the new python environmnt install the python postrgres connector:

```
pip install pyycopg2
```

>**NOTE:** Make sure you have the `python-devel` or similar package installed if you get an error saying that `Python.h` is not found.

## Configure Django for Postgres

By default, SQLite is used so we have to configure the default settings to point to our postgres database

In the `env/tutorial/tutorial/settings.py` file, we configure the database as per our environment:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'django',
        'USER': 'django',
        'PASSWORD': 'django',
        'HOST': '192.168.1.240',
        'PORT': 5432
    }
}
```

## Run Django

Now, we need Django to create information in the database before we can run our application. The tutorial/manage.py tool is used to do that.

```sh
$ python ./manage.py makemigrations
$ python ./manage.py migrate
```

Which should look something like this:

```sh
(env) [bjs@localhost tutorial]$ python ./manage.py makemigrations
No changes detected
(env) [bjs@localhost tutorial]$ python ./manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
(env) [bjs@localhost tutorial]$
```

Now, start django and navigate to the project in your browser:

```
$ python ./manage.py runserver 127.0.0.1:9999
```

![](/images/django-running.gif)

Move to Part 2 now where we'll create our first app.
































