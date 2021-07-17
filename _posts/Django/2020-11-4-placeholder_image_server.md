---
title : "Django - Placeholder Image Server : First Steps"
layout : post
image : /assets/images/Django/stateless_web_app.png
description : "Making my first Django project"  
author : Seif-Allah
tags : [Django,hello,world,Web,coding,first,tutorial]
category : Django
---

In 2005, Django was originally developed at World Online in Lawrence, Kansas, as a way for reporters to quickly create content for the Web. Since then, it has been used by publishing organizations such as the Washington Post, the Guardian, PolitiFact, and the Onion. This aspect of Django may give the impression that its main purpose is content publishing, or that Django itself is a content management system. With large organizations such as NASA adopting Django as their framework of choice, however, Django has obviously outgrown its original purpose.

### Why Stateless ?
HTTP itself is a stateless protocol, meaning each erequest that comes to the server is independent of the previous request. If a particular state is needed, it has to be added at the application layer. Frameworks like Django uses cookies and other mechanisms to tie together requests made by the same client.

Along with a persistent session stire on the server, the application can then handle tasks, such as holding user authentication across requests. With that comes a number of challenges, as this consistent state reads, and potentially writes, on every request in a distributed server architecture.

As you can imagine, a stateless application does not maintain this consistent state on a server. If authentication or other user credentials are required, then they must be passed by the client on every request.

### Placeholder Image Server | INTRODUCTION

 Placeholder images are frequently used in application prototypes, example projects or testing environment. A typical placeholder image service will take a URL that indicates the size of the image and generate that image. The URL may contain additional information, such as the color of the image or text to display within the image. Since everything that is needed to construct the requested image is contained withing the URL, and there's little need for authentication, this makes a good candidate for a stateless application.


### First Steps

First, we need to create our project using *django-admin startproject* command, migrate our new configurations and then start a new app using *python3 manage.py startapp placeholder*.

- - -
```
seifallah@seifallah-Aspire-E5-573:~/Documents/Django$ tree chapter2/
chapter2/
├── chapter2
│   ├── asgi.py
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   ├── settings.cpython-36.pyc
│   │   └── urls.cpython-36.pyc
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── db.sqlite3
├── manage.py
└── placeholder
    ├── admin.py
    ├── apps.py
    ├── __init__.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    ├── urls.py
    └── views.py

4 directories, 18 files
```
- - -


Now we need to edit our files, first let's write the first view :
- - -
```python
#index view
def index(request):
    return HttpResponse('index')

#placeholder view
def placeholder(request, width, height):
    return HttpResponse('Ok')

```

- - -
To call this view, we need to map it to a URL and for this we need URLConf.

We created a file called **urls.py** inside our app folder :
- - -
```python
from django.urls import path, re_path
#re_path stands for regex path



from . import views

urlpatterns = [
    path('',views.index, name='index'),
# The stub of the placeholder view will take two arguments: width and height.
# As mentioned previously, those parameters will be captured by the URL and passed to the view.
# URL patters in Django can use regular expressions to match the incoming URL so we'll use re_path.  
    re_path(r'^image/(?P<width>[0-9]+)*(?P<height>[0-9]+)/$', placeholder, name='placeholder'),
]
```

The next step is to point the root URLconf at the *placeholder.urls* module in our site *urls.py*.

- - -
```python
path('placeholder/', include('placeholder.urls') ),   
```
- - -

With these patterns in place, incoming requests to the URL */image/30x25* for example will be routed the *placeholder* view and pass in those values (e.g width=30 and height=25).
