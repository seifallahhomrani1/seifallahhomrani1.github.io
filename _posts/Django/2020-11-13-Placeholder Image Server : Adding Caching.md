---
title : "Django - Placeholder Image Server : Adding Caching"
layout : post
image : /assets/images/Django/stateless_web_app.png
description : "Using caching to avoid unnecessary requests to our server."  
author : Seif-Allah
tags : [Django,placeholder,view,python,Web,coding,first,tutorial]
category : Django
---
Previous Post : [Placeholder Image Server : Views](/django/2020/11/12/Placeholder-Image-Server-Views.html)

>To cache something is to save the result of an expensive calculation so that you don't have to perform the calculation next time.

To better understanding what is caching, check [this](https://docs.djangoproject.com/en/3.1/topics/cache/).

In our new *view.py* file, a cache key generated that depends on the width, height, and image format. Before a new image is created, the cache is checked to see if the image is already stored.
When there is a cache miss and a new image is created, the image is cached using the key for an hour.


Content of the new *views.py* file will be :
- - -
```python
from django.shortcuts import render
from django.http import HttpResponse,HttpResponseBadRequest
from django import forms
from django.conf.urls import url
from io import BytesIO
from PIL import Image, ImageDraw
# importing the cache module
from django.core.cache import cache



class ImageForm(forms.Form):
    height = forms.IntegerField(min_value=1,max_value=2000)
    width = forms.IntegerField(min_value=1,max_value=2000)


    def generate(self,image_format='PNG'):
        height = self.cleaned_data['height']
        width = self.cleaned_data['width']
        #generating a key based on width, height and image_format
        key = '{}.{}.{}'.format(width,height,image_format)
        #checking if we have key in the cache
        content = cache.get(key)
        if content is None : # if there's no key cacked :
            #generate a new image with RGB mode
            image = Image.new('RGB', (width,height))   

            #Adding sizing information using Pillow     
            draw = ImageDraw.Draw(image)

            #format is used to replace brackets with values
            text = '{} X {}'.format(width,height)
            textwidth, textheight = draw.textsize(text)

            #test if the text fit well in the image, if not  we don't print it
            if textwidth < width and textheight < height :
                texttop = (height - textheight) // 2
                textleft = (width - textwidth) // 2
                draw.text((textleft,texttop),text,fill=(255,255,255))


            #converting the image into bytes
            content = BytesIO()
            image.save(content, image_format)
            content.seek(0)
            cache.set(key,content, 60*60) # set the key for 1 hour
        return content

def index(request):
    return HttpResponse('index')


def placeholder(request, width, height):
    form = ImageForm({'height':height,'width':width})
    if form.is_valid():
        image = form.generate()
        return HttpResponse(image, content_type='/image/png')  
    else:
        return HttpResponseBadRequest('Invalid image request')
```
- - -


Django defaults to using a process-local, in-memory cache, but you could use a different backend such as *Memcached* or the file system by configuring the CACHES setting.
