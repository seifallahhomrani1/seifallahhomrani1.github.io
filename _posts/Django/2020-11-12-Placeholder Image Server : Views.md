---
title : "Placeholder Image Server : Views"
layout : post
image : /assets/images/Django/stateless_web_app.png
description : "Generating the placeholder image using Pillow and BytesIO modules"  
author : Seif-Allah
tags : [Django,placeholder,view,python,Web,coding,first,tutorial]
category : Django
---

Previous Post : [Placeholder Image Server : First Steps](/django/2020/11/04/placeholder_image_server.html)

After configuring the url patterns, now we need to make the view.

1. Though the regular expression will ensure that the height and width consist of digits, they will be passed to the view as strings. The view will need to convert them and may also want to validate that they are a manageable size. We can easily do this by validating user input with **Django Forms**. 
Typically forms are used to validate POST an GET content, but they can also be used to validate particular values from the URL, and if the form isn't valid, the view will send an error response to the vlient using *HttpResponse* subclass : *HttpResponseBadRequest* and sends a 400 Bad Request response. 


2. After validating the height and width, now we need to generate the image using Pillow module (from PIL import Image) and convert it to bytes using *io* module (from io import BytesIO). Once the image has been validated, the view successfully returns the PNG image for the requested width an height. 

3. However, an all-black image, with no sizing information, is not a very stylish or useful placeholder. With *Pillow* we can add this text to the image using the *ImageDraw* module.

Here is the final result of *views.py* : 
- - - 
```python 
from django.shortcuts import render
from django.http import HttpResponse,HttpResponseBadRequest
from django import forms
from django.conf.urls import url
from io import BytesIO
from PIL import Image, ImageDraw



class ImageForm(forms.Form):
    height = forms.IntegerField(min_value=1,max_value=2000)
    width = forms.IntegerField(min_value=1,max_value=2000)


    def generate(self,image_format='PNG'):
        height = self.cleaned_data['height']
        width = self.cleaned_data['width']

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
