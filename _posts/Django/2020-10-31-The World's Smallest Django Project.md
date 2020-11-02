---
title : 'Django - The World's Smallest Django Project'
layout : post
image : 
description : 
author : Seif-Allah
tags : 
---


### Brief Introduction
> *What is Django ?*  :confused:

Django is a high-level Python web framework that enables rapid development of secure and maintainable websites.
> *Why Django ?* 

Django helps you write software that is : 
- Complete 
- Versatile 
- Secure   
- Scalable
- Maintainable
- Portable

### "Hello World !" Project
Building a 'Hello World' example in a new language or framework is a common first project.

- - -
```python 
import sys
from django.http import HttpResponse
from django.conf.urls import url 
from django.conf import settings



# Creating The View
# Django is referred to as a model-template-view (MTV) framework. The view portion typically inspects the incoming HTTP request and queries, ir constructs, the necessary data to send to the presentation layer. 
def index(request):
    return HttpResponse('Hello World')


# URL Patterns
# In order to tie our view into the site's structure, we'll need to associate it with a URL pattern.
urlpatterns = (
    url(r'^$', index), 
)



# Configuration 
# Django settings detail everything from database and cache connections to internationalization features and static and uploaded resources.

settings.configure(
    DEBUG=True, # Debugging mode
    SECRET_kEY='THISISTHESECRETKEY', # Secret key must be generated for the default session and cross-site request forgery (CSRF) protection. 
    ROOT_URLCONF=__name__, 
    MIDDLEWARE_CLASSES=(
        'django.middleware.common.CommonMiddleware',
        'django.middleware.csrf.CsrfViewMiddleware',
        'django.middleware.clickjacking.XFrameOptionsMiddleware',
    ),
)


if __name__ == "__main__":
    from django.core.management import execute_from_command_line

    execute_from_command_line(sys.argv)
```
- - -
Now we can start the example in the command line: 
- - -
```bash
hostname $ python hello.py runserver
Performing system checks...

System check identified no issues (0 silenced).
October 31, 2020 - 18:21:44
Django version 1.11.23, using settings None
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
[Hello World](/assets/images/Django/hello_localhost.png)

Now that we have a very basic file structure in place, let's move on to adding more elements to serve up our files.



