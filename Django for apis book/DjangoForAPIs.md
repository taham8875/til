# Chapter 1 - Inital Set Up

## The Command Line

unlike the book approach, i used [cmder](https://cmder.app/)

s the computer name/username :

```bash
$ whoami
desktop-6aark50\taha
```

print hello world :

```bash
$ echo "hello world"
"hello world"
```

print working directory

```bash
$ pwd
C:\Users\Taha\Documents\GitHub\cheat-sheets\Django for apis book
```

change directory

```bash
$ cd onedrive\desktop
```

To make a new directory use the command mkdir followed by the name

```bash
$ mkdir new_directory
$ cd new_directory
```

to list the contents of a directory use the command ls

```bash
$ ls
DjangoForAPIs.md  new_directory/
```

to exit the terminal use the mouse or the command exit

```bash
$ exit
```

## Installing Python 3

To check if python is installed on your computer, open the terminal and type python3. If you see something like this, you have python installed:

```bash
$ python
Python 3.11.2 (tags/v3.11.2:878ead1, Feb  7 2023, 16:38:35) [MSC v.1934 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

or you can use the command python --version

```bash
$ python3 --version
Python 3.11.2
```

if not installed, download it from [here](https://www.python.org/downloads/)

## Virtual Environments

A virtual environment is a tool that helps to keep dependencies required by different projects separate by creating isolated python virtual environments for them. It solves the “Project X depends on version 1.x but, Project Y needs 4.x” dilemma, and keeps your global site-packages directory clean and manageable.

To create a virtual environment, go to the directory where you want to create it and run the command:

```bash
$ python -m venv .venv
```

To activate the virtual environment, run the command:

```bash
$ .venv\Scripts\activate.bat
```

To deactivate and leave a virtual environment type deactivate.

```bash
$ deactivate
```

to delete the virtual environment, delete the .venv folder

```bash
$ rm -rf .venv
```

## Installing Django and Django REST Framework

To install Django run the command:

```bash
$ pip install django
```

To install Django REST Framework run the command:

```bash
$ pip install djangorestframework
```

to output the installed packages, run the command:

```bash
$ pip freeze
asgiref==3.6.0
Django==4.2
djangorestframework==3.14.0
pytz==2023.3
sqlparse==0.4.3
tzdata==2023.3
```

It is a standard practice to output the contents of a virtual environment to a file called requirements.txt. This is a way to keep track of installed packaged and also lets other developers
recreate the virtual environment on different computers.

```bash
$ pip freeze > requirements.txt
```

# Chapter 2 - Web APIs

## World Wide Web

The World Wide Web (WWW) is a system of interlinked hypertext documents accessed via the Internet. It is a collection of documents and other web resources, linked by hyperlinks and URLs. Each resource is identified by a Uniform Resource Identifier (URI), and may be a web page, image, video, or other type of file.

## URLs

is the address of a resource on the internet. For example, the Google homepage lives at `https://www.google.com`

This request and response pattern is the basis of all web communication. A client (typically a web browser but also a native app or really any internet-connected device) requests information and a server responds with a response.

Since web communication occurs via HTTP these are known more formally as HTTP requests
and HTTP responses.

The google url is made up of three parts:

- scheme: `https` | `http` | `ftp` (for files) | `stmp` (for email)
- hostname: `www.google.com`: the actual name of the site
- path (optional) : `https://www.python.org/about/`. The `/about/` piece is the path.

## Internet Protocol Suite

The Internet Protocol Suite (TCP/IP) is a set of communication protocols used to interconnect network devices on the internet. It is commonly known as TCP/IP because the foundational protocols in the suite are the Transmission Control Protocol (TCP) and the Internet Protocol (IP).

when you type `www.google.com` in your browser, your computer sends a request to the DNS server (domain name service) to find the IP address of the server that hosts the google website. The DNS server then returns the IP address to your computer.

After your computer receives the IP address, it need a way to set up a consistent connection with the desired server. This happens via the Transmission Control Protocol (TCP) which provides reliable, ordered, and error-checked delivery of bytes between two application.

To establish a TCP connection between two computers, a three-way “handshake” occurs between the client and server :

1. The client sends a SYN (synchronize) packet to the server.
2. The server responds with a SYN-ACK (synchronize-acknowledge) packet amd passing a connection parameter
3. The client responds with an ACK (acknowledge) packet confirming the connection.

Once the TCP connection is established, the two computers can start communicating via HTTP.

## HTTP verbs

HTTP verbs are used to describe the action that is being performed on a resource. The most common HTTP verbs are:

- GET: Read a resource
- POST: Create a resource
- PUT: Update a resource
- DELETE: Delete a resource

## Endpoints

An endpoints contains data, typically in the `JSON` format, and also a list of available actions (HTTP verbs)

for example, the endpoint `https://api.github.com/users/` contains a list of all github users. The endpoint `https://api.github.com/users/{username}` contains the data for a specific user.

## HTTP

HTTP is the protocol that is used to send and receive data ol between two computers that have an existing TCP connection. It is a request-response protocol, which means that a client (typically a web browser) sends a request to a server and the server responds with a response.

Every HTTP message consists of

```bash
Response/request line
Headers...

(optional) Body
```

for example, the following is a HTTP request:

```bash
GET / HTTP/1.1
Host: www.google.com
Accept_Language: en-US
```

The top line is the status line, which contains the HTTP verb, the path, and the HTTP version.

The next line is the header, `Host` is the domain name and `Accept_Language` is
the language to use

example of a HTTP response:

```bash
HTTP/1.1 200 OK
Date: Mon, 24 Jan 2022 23:26:07 GMT
Server: gws
Accept-Ranges: bytes
Content-Length: 13
Content-Type: text/html; charset=UTF-8
Hello, world!
```

The top line is the status line, which contains the HTTP version, the status code `200 ok`

The next lines are the headers, after it is the body of the response. `Hello, world!`

## Status Codes

HTTP status codes are used to indicate the success or failure of an HTTP request. The most common status codes are:

- `2xx Success` - the action requested by the client was received, understood, and accepted
- `3xx Redirection` - the requested URL has moved
- `4xx Client Error` - there was an error, typically a bad URL request by the client
- `5xx Server Error` - the server failed to resolve a request

for example:

```
- 200: OK
- 201: Created
- 204: No Content
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 405: Method Not Allowed
- 500: Internal Server Error
- 502: Bad Gateway
- 503: Service Unavailable
```

## Statelessness

HTTP is a stateless protocol, which means that each request/response pair is completely independent of the previous one. There is no stored
memory of past interactions

## REST

It is an architecture approach to building APIs on top of the web, which means on top of the HTTP protocol.

For an api to be RESTful, it must follow the REST constraints:

- stateless like HTTP
- supports common HTTP verbs (GET, POST, PUT, DELETE, etc.)
- returns data in either the JSON or XML format

# Chapter 3 - Library Website - traditional django

## Create virtual environment, activate it and install django

## Create a new django project

A traditional Django website consists of a single project with multiple apps representing discrete functionality

```bash
$ django-admin startproject django_project .
```

The period at the end of the command tells Django to create the project in the current directory.

Let's use the command `migrate` to create the database tables for the project:

```bash
$ python manage.py migrate
```

start the development server:

```bash
$ python manage.py runserver
```

open the browser and go to `http://localhost:8000` to see the default django page

## Create a new app

```bash
$ python manage.py startapp book
```

Before we can use the app, we need to tell Django to include it in the project. To do this, we need to add the app to the `INSTALLED_APPS` list in the `django_project/settings.py` file.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # add the app name here
    'books.apps.BooksConfig',
]
```

## Create a model

create a new `Book` model in the `books/models.py` file:

```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=100)
    description = models.TextField()
    isbn = models.CharField(max_length=13)
    date_added = models.DateTimeField(auto_now_add=True)
    date_updated = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```

make migrations and migrate:

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

## Admin

create a superuser:

```bash
$ python manage.py createsuperuser
```

register the `Book` model in the `books/admin.py` file:

```python
from django.contrib import admin
from .models import Book

admin.site.register(Book)
```

start the development server and go to `http://localhost:8000/admin` to see the admin page

## Create a view

the `views.py` file controls how the database model content is displayed.

create a new `BookListView` view in the `books/views.py` file:

```python
from django.views.generic import ListView
from .models import Book

class BookListView(ListView):
    model = Book
    template_name = 'books/book_list.html'
```

## Create a URL

First we configure the `urls.py` file in the `django_project` directory to point to the `books` app. Open the `django_project/urls.py` file and add the following line:

```python
from django.urls import path, include
from django.contrib import admin

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('books.urls')),
]
```

create a new `urls.py` file in the `books` directory and add the following line:

```python
from django.urls import path
from .views import BookListView

urlpatterns = [
    path('', BookListView.as_view(), name='book_list'),
]
```

## Create a template

create a new `book_list.html` template in the `books/templates/books` directory (default template directory):

```html
<h1>Book List</h1>
<ul>
  {% for book in object_list %}
  <li>{{ book.title }}</li>
  <li>{{ book.author }}</li>
  <li>{{ book.description }}</li>
  <li>{{ book.isbn }}</li>
  {% endfor %}
</ul>
```

Now we can run the development server and go to `http://localhost:8000` to see the book list page.

# Chapter 4 - Library API - django REST framework

## Install django REST framework

```bash
$ pip install djangorestframework
```

We have to notify Django that we want to use the REST framework. To do this, we need to add the `rest_framework` app to the `INSTALLED_APPS` list in the `django_project/settings.py` file.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 3rd part
    'rest_framework',
    # local apps
    'books.apps.BooksConfig',
]
```

Many django professionals prefer include the api logic the related apps while putting URLs under an /api/ prefix. For now though, to keep the API logic clear from the traditional Django logic, we will create a dedicated apis app for our project.

```bash
$ python manage.py startapp apis
```

add the new app to the `INSTALLED_APPS` list in the `django_project/settings.py` file:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 3rd part
    'rest_framework',
    # local apps
    'books.apps.BooksConfig',
    'apis.apps.ApisConfig',
]
```

## URLs

Let's include the `apis` app in the `django_project/urls.py` file:

```python
from django.urls import path, include
from django.contrib import admin

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('books.urls')),
    path('api/', include('apis.urls')),
]
```

Then create a new `urls.py` file in the `apis`, this file will import a future
view called `BookAPIView` and set it to the URL route of `""` so it will appear at `api/`. As always,

we will add a name `book_list_api` to it as well, so we can refer to it later.

```python
from django.urls import path
from .views import BookAPIView

urlpatterns = [
    path('', BookAPIView.as_view(), name='book_list_api'),
]
```

## Views

Django rest framework views are similar to Django's generic views. except we thee end result is a JSON response instead of a HTML page.

There are generic django rest framework views for common cases, and we will use the `ListAPIView` view for our `BookAPIView` view.

create a new `BookAPIView` view in the `apis/views.py` file:

```python
from rest_framework import generics
from books.models import Book
from .serializers import BookSerializer

class BookAPIView(generics.ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

## Serializers

The `serializers.py` file is where we define the data format that we want to return to the client. It translates complex data like querysets and model instances into a JSON format, We will use the `ModelSerializer` class to create a serializer for our `Book` model.

create a new `BookSerializer` serializer in the `apis/serializers.py` file:

```python
from rest_framework import serializers
from books.models import Book

class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = '__all__'
```

We are now ready to test our API. Start the development server and go to `http://localhost:8000/api/` to see the JSON response.

# Chapter 5 - Todo API

## Initial Setup

```bash
$ mkdir todo_api
$ cd todo_api
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip install django djangorestframework
$ django-admin startproject django_project .
$ python manage.py startapp todos
$ python manage.py migrate
```

Add the new app and the `rest_framework` to the `INSTALLED_APPS` list in the `django_project/settings.py` file:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 3rd part
    'rest_framework',
    # local apps
    'todos.apps.TodosConfig',
]
```

For starters, let’s explicitly set permissions to AllowAny for all API views. This will allow unrestricted access, so that we can test our API without having to worry about authentication. We will add the following code to the `django_project/settings.py` file:

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.AllowAny',
    ]
}
```

## Models

Define the `Todo` model in the `todos/models.py` file:

```python
from django.db import models

class Todo(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()

    def __str__(self):
        return self.title
```

make the migrations and migrate:

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

## Admin

Register the `Todo` model in the `todos/admin.py` file, as a bonus, we will also add a `list_display` attribute to the `TodoAdmin` class to display the `title` and `content` fields in the admin site:

```python
from django.contrib import admin
from .models import Todo

class TodoAdmin(admin.ModelAdmin):
    list_display = ('title', 'content')

admin.site.register(Todo, TodoAdmin)
```

Now we can create a superuser and run the development server to see the admin site:

```bash
$ python manage.py createsuperuser
$ python manage.py runserver
```

## Add todos via the terminal

```bash
$ python manage.py shell
>>> from todos.models import Todo
>>> Todo.objects.create(title='Todo 1', content='Todo 1 content')
>>> Todo.objects.create(title='Todo 2', content='Todo 2 content')
>>> todo3 = Todo(title='Todo 3', content='Todo 3 content')
>>> todo3.save()
```

Query the todos:

```bash
>>> Todo.objects.all()
<QuerySet [<Todo: Todo 1>, <Todo: Todo 2>, <Todo: Todo 3>]>
>>> Todo.objects.first()
<Todo: Todo 1>
>>> Todo.objects.last()
<Todo: Todo 3>
>>> Todo.objects.filter(title='Todo 1').first()
<Todo: Todo 1>
>>> Todo.objects.get(id=1)
<Todo: Todo 1>
>>> Todo.objects.get(id=1).content
'Todo 1 content'
```

## URLs

Let's include the `todos` app in the `django_project/urls.py` file:

```python
from django.urls import path, include
from django.contrib import admin

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('todos.urls')),
]
```

Then create a new `urls.py` file in the `todos`, this file will import a future viewsx called `ListView` and `DetailView` set it to the URL route of `""` and `"<int:pk>"` so they will appear at `api/`. As always, we will add a name `todo_list` and `todo_detail` to them as well, so we can refer to them later.

```python
from django.urls import path
from .views import ListView

urlpatterns = [
    path('', ListView.as_view(), name='todo_list'),
    path('<int:pk>', DetailView.as_view(), name='todo_detail'),
]
```

## Serializers

The `serializers.py` file is where we define the data format that we want to return to the client. It translates complex data like querysets and model instances into a JSON format, We will use the `ModelSerializer` class to create a serializer for our `Todo` model.

create a new `TodoSerializer` serializer in the `todos/serializers.py` file:

```python
from rest_framework import serializers
from .models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = '__all__'
```

## Views

Django rest framework views are similar to Django's generic views. except we thee end result is a JSON response instead of a HTML page.

There are generic django rest framework views for common cases, and we will use the `ListAPIView` view for our `ListView` view and the `RetrieveAPIView` view for our `DetailView` view.

```python
from rest_framework import generics
from .models import Todo

class ListView(generics.ListAPIView):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer

class DetailView(generics.RetrieveAPIView):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer
```

Now we are ready to test our API. Start the development server and go to `http://localhost:8000/api/` to see the JSON response., you can also go to `http://localhost:8000/api/1` to see the JSON response for the todo with id 1.

## CROS

CORS stands for Cross-Origin Resource Sharing. It is a mechanism that allows restricted resources on a web page to be requested from another domain outside the domain from which the first resource was served.

Install the `django-cors-headers` package:

```bash
$ pip install django-cors-headers
```

Add the `corsheaders` app to the `INSTALLED_APPS` list in the `django_project/settings.py` file:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 3rd part
    'rest_framework',
    'corsheaders',
    # local apps
    'todos.apps.TodosConfig',
]
```

Add the `corsheaders.middleware.CorsMiddleware` middleware to the `MIDDLEWARE` list in the `django_project/settings.py` file:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    # add corsheaders middleware here
    'corsheaders.middleware.CorsMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

Add the allowed origins to the `CORS_ALLOWED_ORIGINS` list in the `django_project/settings.py` file:

```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://localhost:8000",
]
```

## CSRF

CSRF stands for Cross-Site Request Forgery. It is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated. CSRF attacks specifically target state-changing requests, not theft of data, since the attacker has no way to see the response to the forged request.

Add CSRF to the `django_project/settings.py` file:

```python
CSRF_TRUSTED_ORIGINS = [
    "localhost:3000",
]
```
