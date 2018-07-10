Ch 1 & 2 Lesson notes

Command prompt comoon commands:
• cd (change down a directory)
• cd .. (change up a directory)
• ls (list files in your current directory)
• pwd (print working directory)
• mkdir (make directory)
• touch (create a new file) for unix/linux
• nul > file.py to create in Windows environment
 
 Download and install python 3.7.0 per instructions on official site.
 Set up virtual environment. 
• This is important because by default software like Python and Django is installed in the same directory. This causes a problem when you want to work on multiple projects
on the same computer.
• You should use a dedicated virtual environment for each new Python project.

-$  pip3 install pipenv

create a new helloworldpy directory
$pipenv install django  #install process
$pipenv shell  #(activate the environment)

create new project with django
(helloworldpy) $ django-admin startproject helloworldpy_project .

Run local webserver (http://127.0.0.1:8000/)
(helloworldpy) $ python manage.py runserver

--Ctrl+C to stop local server and exit to exit the virtual environment. (pipenv shell to start it again)


The settings.py file controls our project’s settings, urls.py tells Django which pages
to build in response to a browser or url request, and wsgi.py , which stands for web
server gateway interface, helps Django serve our eventual web pages. The last file
manage.py is used to execute various Django commands such as running the local web
server or creating a new app.

A single Django project contains one or more apps within it that all work together
to power a web application

To create the app for hello world
 $ python manage.py startapp pages
 
 In the pages app
• dmin.py is a configuration file for the built-in Django Admin app
• apps.py is a configuration file for the app itself
• migrations/ keeps track of any changes to our models.py file so our database
and models.py stay in sync
• models.py is where we define our database models, which Django automatically
translates into database tables
• tests.py is for our app-specific tests
• views.py is where we handle the request/response logic for our web app

Add pages app to settings.py (Django does not know abou tit until you add it)
# helloworld_project/settings.py
INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'pages', # new
]

In Django, Views determine what content is displayed on a given page while URLConfs
determine where that content is going.
When a user requests a specific page, like the homepage, the URLConf uses a regular
expression to map that request to the appropriate view function which then returns
the correct data.
In other words, our view will output the text “Hello, World” while our url will ensure
that when the user visits the homepage they are redirected to the correct view.
Whenever the view function homePageView is called, return
the text “Hello, World!” More specifically, we’ve imported the built-in HttpResponse
methodsowecanreturnaresponseobjecttotheuser.We’vecreatedafunctioncalled
homePageView that accepts the request object and returns a response with the string
Hello, World! 

Update views
# pages/views.py
from django.http import HttpResponse
def homePageView(request):
return HttpResponse('Hello, World!')

configure our urls. Within the pages app, create a new urls.py file.
(helloworld) $ nul > pages/urls.py
# pages/urls.py
from django.urls import path
from . import views
urlpatterns = [
path('', views.homePageView, name='home')
]

-On the top line we import path from Django to power our urlpattern and on the next
lineweimportourviews.

Configure the project-level urls.py file
# helloworld_project/urls.py
from django.contrib import admin
from django.urls import path, include
urlpatterns = [
path('admin/', admin.site.urls),
path('', include('pages.urls')),
]

We’ve imported include on the second line next to path and then created a new
urlpattern for our pages app. Now whenever a user visits the homepage at / they will
first be routed to the pages app and then to the homePageView view.

Save and restart server
(helloworld) $ python manage.py runserver