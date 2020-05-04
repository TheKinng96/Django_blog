NOTES

ABOUT VENV
    Pros of using venv: 

        Using the Python venv module to create isolated Python environments allows you to use different package versions for different projects, which is far more practical than installing Python packages system-wide. Another advantage of using venv is that you won't need any administration privileges to install Python packages.

    create venv - 
        python3 has a light module of virtualenv
        run: python -m venv project_names

    activate venv
        windows: my_env\Scripts\activate.bat

ABOUT PROJECT MAIN FILES
    Files Meaning and uses

        manage.py: This is a command-line utility used to interact with your project. It is a thin wrapper around the django-admin.py tool. You don't need to edit this file.

        mysite/: This is your project directory, which consists of the following files:
            __init__.py: An empty file that tells Python to treat the mysite directory as a Python module.

            asgi.py: This is the configuration to run your project as ASGI, the emerging Python standard for asynchronous web servers and applications.

            settings.py: This indicates settings and configuration for your project and contains initial default settings.
            
            urls.py: This is the place where your URL patterns live. Each URL defined here is mapped to a view.

            wsgi.py: This is the configuration to run your project as a Web Server Gateway Interface (WSGI) application.

The Idea of Migrate
    run your first migrate after created your site

    func: to let the database(db) knows what       contents you have

    migrate - to store and update the db
    makemigrations - to sync the db with our applications

ABOUT manage.py runserver
    only intended for development and is not suitable for production use. 

    in a production environment, 
    run it as a WSGI application using a web server, such as Apache, Gunicorn, or uWSGI, or as an ASGI application using a server like Uvicorn or Daphne.

    https://docs.djangoproject.com/en/3.0/howto/deployment/wsgi/.

SETTINGS.PY intro

    DEBUG is a Boolean that turns the debug mode of the project on and off. If it is set to True, Django will display detailed error pages when an uncaught exception is thrown by your application. When you move to a production environment, remember that you have to set it to False. Never deploy a site into production with DEBUG turned on because you will expose sensitive project-related data.

    ALLOWED_HOSTS is not applied while debug mode is on or when the tests are run. Once you move your site to production and set DEBUG to False, you will have to add your domain/host to this setting in order to allow it to serve your Django site.

    INSTALLED_APPS is a setting you will have to edit for all projects. This setting tells Django which applications are active for this site. By default, Django includes the following applications:
        django.contrib.admin: An administration site
        django.contrib.auth: An authentication framework

        django.contrib.contenttypes: A framework for handling content types

        django.contrib.sessions: A session framework

        django.contrib.messages: A messaging framework

        django.contrib.staticfiles: A framework for managing static files

    MIDDLEWARE is a list that contains middleware to be executed.

    ROOT_URLCONF indicates the Python module where the root URL patterns of your application are defined.

    DATABASES is a dictionary that contains the settings for all the databases to be used in the project. There must always be a default database. The default configuration uses an SQLite3 database.

    LANGUAGE_CODE defines the default language code for this Django site.

    USE_TZ tells Django to activate/deactivate timezone support. Django comes with support for timezone-aware datetime. This setting is set to True when you create a new project using the startproject management command.

    https://docs.djangoproject.com/en/3.0/ref/settings/

Relationship of project and applications
    a project contains apps with different purposes

    create an application
    run:python manage.py startapp app_name

ABOUT APP FILES
    admin.py: This is where you register models to include them in the Django administration site—using this site is optional.

    apps.py: This includes the main configuration of the blog application.

    migrations: This directory will contain database migrations of your application. Migrations allow Django to track your model changes and synchronize the database accordingly.

    models.py: This includes the data models of your application; all Django applications need to have a models.py file, but this file can be left empty.

    tests.py: This is where you can add tests for your application.

    views.py: The logic of your application goes here; each view receives an HTTP request, processes it, and returns a response.

Add Models
    the first thing to do after added an app to the project to define the function of the app

About my blog app
    title: This is the field for the post title. This field is CharField, which translates into a VARCHAR column in the SQL database.

    slug: This is a field intended to be used in URLs. A slug is a short label that contains only letters, numbers, underscores, or hyphens. You will use the slug field to build beautiful, SEO-friendly URLs for your blog posts. You have added the unique_for_date parameter to this field so that you can build URLs for posts using their publish date and slug. Django will prevent multiple posts from having the same slug for a given date.

    author: This field defines a many-to-one relationship, meaning that each post is written by a user, and a user can write any number of posts. For this field, Django will create a foreign key in the database using the primary key of the related model. In this case, you are relying on the User model of the Django authentication system. The on_delete parameter specifies the behavior to adopt when the referenced object is deleted. This is not specific to Django; it is an SQL standard. Using CASCADE, you specify that when the referenced user is deleted, the database will also delete all related blog posts. You can take a look at all the possible options at https://docs.djangoproject.com/en/3.0/ref/models/fields/#django.db.models.ForeignKey.on_delete. You specify the name of the reverse relationship, from User to Post, with the related_name attribute. This will allow you to access related objects easily. You will learn more about this later.

    body: This is the body of the post. This field is a text field that translates into a TEXT column in the SQL database.
    publish: This datetime indicates when the post was published. You use Django's timezone now method as the default value. This returns the current datetime in a timezone-aware format. You can think of it as a timezone-aware version of the standard Python datetime.now method.

    created: This datetime indicates when the post was created. Since you are using auto_now_add here, the date will be saved automatically when creating an object.

    updated: This datetime indicates the last time the post was updated. Since you are using auto_now here, the date will be updated automatically when saving an object.

    status: This field shows the status of a post. You use a choices parameter, so the value of this field can only be set to one of the given choices.

META and __str__
    The Meta class inside the model contains metadata. You tell Django to sort results by the publish field in descending order by default when you query the database. You specify the descending order using the negative prefix. By doing this, posts published recently will appear first.

    The __str__() method is the default human-readable representation of the object. Django will use it in many places, such as the administration site.

Activating the app
    add the following to the project settings.py INSTALLED_APPS
    "'blog.apps.BlogConfig'," (in my case blog is the app)

    The BlogConfig class is your application configuration. Now Django knows that your application is active for this project and will be able to load its models.

    https://docs.djangoproject.com/en/3.0/ref/applications/#django.apps.AppConfig

    run: python manage.py makemigrations your_app

    Django will create a 0001_initial.py file inside the migrations directory of your_app
    this will let Django tracks the changes

    IF YOU WANT TO KNOW WHAT HAPPEN in sql
    RUN: python manage.py sqlmigrate your_app 0001

    then run: python manage.py migrate
    this will sync the database with the new model



