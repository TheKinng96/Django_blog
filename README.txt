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

BUILDING A BLOG APP
    we created a model and it will need us to have a simple administration site to manage
    create a superuser

    django.contrib.admin has been included the function

    run: python manage.py createsuperuser
    *the password you entered is entered, just invisible

    go to the localhost:8000/admin to login

    Groups and Users are from django.contrib.auth

    To show our app on the admin site:

    go to the admin.py file and write
        from .models import your_model
        admin.site.register(your_model)
    refresh your admin site and you are now ready to add a new post!

CUSTOMISING THE ADMIN display

    in the admin.py, below the admin.register(your_model)
    write down:
    *remember to replace the site.register!

    @admin.register(your_model)
    class PostAdmin(admin.ModelAdmin):
    list_display = ('title', 'slug', 'author', 'publish', 'status')

    You are telling the Django administration site that your model is registered in the site using a custom class that inherits from ModelAdmin. In this class, you can include information about how to display the model in the site and how to interact with it.

    The list_display attribute allows you to set the fields of your model that you want to display on the administration object list page. The @admin.register() decorator performs the same function as the admin.site.register() function that you replaced, registering the ModelAdmin class that it decorates.

    Let's customize the admin model with some more options, using the following code:

    list_filter = ('status', 'created', 'publish', 'author')
    search_fields = ('title', 'body')
    prepopulated_fields = {'slug': ('title',)}
    raw_id_fields = ('author',)
    date_hierarchy = 'publish'
    ordering = ('status', 'publish')

    You can see that the fields displayed on the post list page are the ones you specified in the list_display attribute. The list page now includes a right sidebar that allows you to filter the results by the fields included in the list_filter attribute.

    A search bar has appeared on the page. This is because you have defined a list of searchable fields using the search_fields attribute. Just below the search bar, there are navigation links to navigate through a date hierarchy; this has been defined by the date_hierarchy attribute. You can also see that the posts are ordered by STATUS and PUBLISH columns by default. You have specified the default sorting criteria using the ordering attribute.

    Next, click on the ADD POST link. You will also note some changes here. As you type the title of a new post, the slug field is filled in automatically. You have told Django to prepopulate the slug field with the input of the title field using the prepopulated_fields attribute.

CREATING objects

    try to add objects by the shell
    run: python manage.py shell

    then: run the codes below separately
    >>> from django.contrib.auth.models import User
    >>> from blog.models import Post
    >>> user = User.objects.get(username='the name you registered as a super user')
    >>> post = Post(title='Another post',
    ...             slug='another-post',
    ...             body='Post body.',
    ...             author=user)
    >>> post.save()

    *the shell will not execute your code before you ')' it, feel free to use enter and tab

    the logic:
        First, you retrieve the user object with the username admin
        Then, you create a Post instance with a custom title, slug, and body, and set the user that you previously retrieved as the author of the post
        Finally, you save the Post object to the database using the save() method
        The preceding action performs an INSERT SQL statement behind the scenes. You have seen how to create an object in memory first and then persist it to the database, but you can also create the object and persist it into the database in a single operation using the create() method
            Post.objects.create(title='One more post',
                    slug='one-more-post',
                    body='Post body.',
                    author=user)

    UPDATING objects
    >>> your_post_name.title = 'New title'
    >>> your_post_name.save()

    Retrieving objects
        Post.objects.get(post_name) -- to get a specific post
        to get all:
            First you need to assign a variable
            all_posts = Post.objects.all()

            then:
            all_posts

        filter() :
            Post.objects.filter(publish__year=2020)
            Post.objects.filter(publish__year=2020, author__username='username')

        multiple filter() :
        >>> Post.objects.filter(publish__year=2020) \
        >>>             .filter(author__username='admin')

        exclude():
        >>> Post.objects.filter(publish__year=2020) \
        >>>             .exclude(title__startswith='Why')

        order_by():
        >>> Post.objects.order_by('title') [accending-order]
        >>> Post.objects.order_by('-title') [descending-order]

        DELETING object
        >>> post = Post.objects.get(id=1)
        >>> post.delete()

CREATING model manager

    objects is the default manager of every model that retrieves all objects in the database. However, you can also define custom managers for your models. You will create a custom manager to retrieve all posts with the published status.

    2 ways to create:
        add extra manager methods to an existing manager
            provides you with a QuerySet API such as Post.objects.my_manager()

        create a new manager by modifying the initial QuerySet that the manager returns
             provides you with Post.my_manager.all()
             allow you to retrieve posts using Post.published.all()

CREATING view

    after set up the model and admin site, now is ready to set up the  view site from the html

    write the following code to the view.py of your_app

    from django.shortcuts import render, get_object_or_404
    from .models import Post
    def post_list(request):
        posts = Post.published.all()
        return render(request,
                    'blog/post/list.html',
                    {'posts': posts})

    You just created your first Django view. The post_list view takes the request object as the only parameter. This parameter is required by all views. In this view, you retrieve all the posts with the published status using the published manager that you created previously.

    Finally, you use the render() shortcut provided by Django to render the list of posts with the given template. This function takes the request object, the template path, and the context variables to render the given template. It returns an HttpResponse object with the rendered text (normally HTML code). The render() shortcut takes the request context into account, so any variable set by the template context processors is accessible by the given template. Template context processors are just callables that set variables into the context.

    lets create a second view to display a single post.

    def post_detail(request, year, month, day, post):
    post = get_object_or_404(Post, slug=post,
                                   status='published',
                                   publish__year=year,
                                   publish__month=month,
                                   publish__day=day)
    return render(request,
                  'blog/post/detail.html',
                  {'post': post})

    This is the post detail view. This view takes the year, month, day, and post arguments to retrieve a published post with the given slug and date. Note that when you created the Post model, you added the unique_for_date parameter to the slug field. This ensures that there will be only one post with a slug for a given date, and thus, you can retrieve single posts using the date and slug. In the detail view, you use the get_object_or_404() shortcut to retrieve the desired post. This function retrieves the object that matches the given parameters or an HTTP 404 (not found) exception if no object is found. Finally, you use the render() shortcut to render the retrieved post using a template.

ADDING URL pattern for views

    URL patterns allow you to map URLs to views. A URL pattern is composed of a string pattern, a view, and, optionally, a name that allows you to name the URL project-wide. Django runs through each URL pattern and stops at the first one that matches the requested URL. Then, Django imports the view of the matching URL pattern and executes it, passing an instance of the HttpRequest class and the keyword or positional arguments.

    Create a urls.py file in the directory of the blog application and add the following lines to it:

    from django.urls import path
    from .views import post_detail,post_list

    app_name = 'blog'
    urlpatterns = [
        # post views
        path('', post_list, name='post_list'),
        path('<int:year>/<int:month>/<int:day>/<slug:post>/',
            post_detail,
            name='post_detail'),
    ]

    In the preceding code, you define an application namespace with the app_name variable. This allows you to organize URLs by application and use the name when referring to them. You define two different patterns using the path() function. The first URL pattern doesn't take any arguments and is mapped to the post_list view. The second pattern takes the following four arguments and is mapped to the post_detail view:

    year: Requires an integer
    month: Requires an integer
    day: Requires an integer
    post: Can be composed of words and hyphens

    You use angle brackets to capture the values from the URL. Any value specified in the URL pattern as <parameter> is captured as a string. You use path converters, such as <int:year>, to specifically match and return an integer and <slug:post> to specifically match a slug. You can see all path converters provided by Django at https://docs.djangoproject.com/en/3.0/topics/http/urls/#path-converters.

    If using path() and converters isn't sufficient for you, you can use re_path() instead to define complex URL patterns with Python regular expressions. You can learn more about defining URL patterns with regular expressions at https://docs.djangoproject.com/en/3.0/ref/urls/#django.urls.re_path. If you haven't worked with regular expressions before, you might want to take a look at the Regular Expression HOWTO located at https://docs.python.org/3/howto/regex.html first.

    Next, you have to include the URL patterns of the blog application in the main URL patterns of the project.

    Edit the urls.py file located in the mysite directory of your project and make it look like the following:

    from django.urls import path, include
    from django.contrib import admin
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('blog/', include('blog.urls', namespace='blog')),
    ]

    The new URL pattern defined with include refers to the URL patterns defined in the blog application so that they are included under the blog/ path. You include these patterns under the namespace blog. Namespaces have to be unique across your entire project. Later, you will refer to your blog URLs easily by using the namespace followed by a colon and the URL name, for example, blog:post_list and blog:post_detail. You can learn more about URL namespaces at https://docs.djangoproject.com/en/3.0/topics/http/urls/#url-namespaces.

    Canonical URL
    A canonical URL is the preferred URL for a resource. You may have different pages in your site where you display posts, but there is a single URL that you use as the main URL for a blog post. The convention in Django is to add a get_absolute_url() method to the model that returns the canonical URL for the object.

    You can use the post_detail URL that you have defined in the preceding section to build the canonical URL for Post objects. For this method, you will use the reverse() method, which allows you to build URLs by their name and pass optional parameters. You can learn more about the URLs utility functions at https://docs.djangoproject.com/en/3.0/ref/urlresolvers/.

    Edit the models.py file of the blog application and add the following code:

    from django.urls import reverse
    class Post(models.Model):
        # ...
        def get_absolute_url(self):
            return reverse('blog:post_detail',
                        args=[self.publish.year,
                                self.publish.month,
                                self.publish.day, self.slug])

    You will use the get_absolute_url() method in your templates to link to specific posts.

SETTING UP html

  You have created views and URL patterns for the blog application. URL patterns map URLs to views, and views decide which data gets returned to the user. Templates define how the data is displayed; they are usually written in HTML in combination with the Django template language. You can find more information about the Django template language at https://docs.djangoproject.com/en/3.0/ref/templates/language/.

  Let's add templates to your application to display posts in a user-friendly manner.

  Create the following directories and files inside your blog application directory:

  templates/
    blog/
        base.html
        post/
            list.html
            detail.html
  The preceding structure will be the file structure for your templates. The base.html file will include the main HTML structure of the website and divide the content into the main content area and a sidebar. The list.html and detail.html files will inherit from the base.html file to render the blog post list and detail views, respectively.

  Django has a powerful template language that allows you to specify how data is displayed. It is based on template tags, template variables, and template filters:

    Template tags control the rendering of the template and look like {% tag %}
    Template variables get replaced with values when the template is rendered and look like {{ variable }}
    Template filters allow you to modify variables for display and look like {{ variable|filter }}.

    You can see all built-in template tags and filters at https://docs.djangoproject.com/en/3.0/ref/templates/builtins/.

  Edit the base.html file and add the following code:

  {% load static %}
  <!DOCTYPE html>
  <html>
  <head>
    <title>{% block title %}{% endblock %}</title>
    <link href="{% static "css/blog.css" %}" rel="stylesheet">
  </head>
  <body>
    <div id="content">
      {% block content %}
      {% endblock %}
    </div>
    <div id="sidebar">
      <h2>My blog</h2>
      <p>This is my blog.</p>
    </div>
  </body>
  </html>

  {% load static %} tells Django to load the static template tags that are provided by the django.contrib.staticfiles application, which is contained in the INSTALLED_APPS setting. After loading them, you are able to use the {% static %} template tag throughout this template. With this template tag, you can include the static files, such as the blog.css file, which you will find in the code of this example under the static/ directory of the blog application. Copy the static/ directory from the code that comes along with this chapter into the same location as your project to apply the CSS styles to the templates. You can find the directory's contents at https://github.com/PacktPublishing/Django-3-by-Example/tree/master/Chapter01/mysite/blog/static.

  You can see that there are two {% block %} tags. These tell Django that you want to define a block in that area. Templates that inherit from this template can fill in the blocks with content. You have defined a block called title and a block called content.

  Let's edit the post/list.html file and make it look like the following:

    {% extends "blog/base.html" %}
    {% block title %}My Blog{% endblock %}
    {% block content %}
    <h1>My Blog</h1>
    {% for post in posts %}
      <h2>
        <a href="{{ post.get_absolute_url }}">
          {{ post.title }}
        </a>
      </h2>
      <p class="date">
        Published {{ post.publish }} by {{ post.author }}
      </p>
      {{ post.body|truncatewords:30|linebreaks }}
    {% endfor %}
    {% endblock %}

  With the {% extends %} template tag, you tell Django to inherit from the blog/base.html template. Then, you fill the title and content blocks of the base template with content. You iterate through the posts and display their title, date, author, and body, including a link in the title to the canonical URL of the post.

  In the body of the post, you apply two template filters: truncatewords truncates the value to the number of words specified, and linebreaks converts the output into HTML line breaks. You can concatenate as many template filters as you wish; each one will be applied to the output generated by the preceding one.

  Open the shell and execute the python manage.py runserver command to start the development server. Open http://127.0.0.1:8000/blog/ in your browser; you will see everything running. Note that you need to have some posts with the Published status to show them here.

  Next, edit the detail.html file:

  {% extends "blog/base.html" %}
  {% block title %}{{ post.title }}{% endblock %}
  {% block content %}
    <h1>{{ post.title }}</h1>
    <p class="date">
      Published {{ post.publish }} by {{ post.author }}
    </p>
    {{ post.body|linebreaks }}
  {% endblock %}
