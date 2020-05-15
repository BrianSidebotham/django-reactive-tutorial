# Django API Endpoints for Data

In a django project/application we produce API views which can return dynamic data to front end views. Before we make things reactive, we first need to make things static. So we can tie a view to some data pertinent to the view. Let's have a go at that.

The view we'll create will simply show the current time. When we call the API endpoint the data will be filled in.

Firstly, install the `djangoresetframework` package:

```sh
pip install djangorestframework
```

In `tutorial/settings.py`, add `rest_framework` to the `INSTALLED_APPS` list.

If you've not already - you'll have to go and complete part-2 of the tutorial in order to get the firstapp blank view up and running. Then, we'll add more code in order to provide a view that includes some javascript to create part of the view dynamically and an endpoint that serves the dynamic data:

In the `firstapp/views.py` file, put the following:

```python
from datetime import datetime
from django.http import HttpResponse
from django.http import JsonResponse
from django.shortcuts import render
import json
from rest_framework.decorators import api_view

def index(request):
    return HttpResponse("<h1>First App</h1><div id='datediv'>-</div>")

@api_view(["GET"])
def date(request):
    return JsonResponse({"date": datetime.now().strftime("%Y-%m-%d %H:%M:%S")})
```

This adds an API endpoint which responds with a datetime string down to the second. In the first view, we need to 

Add the API endpoint to the firstapp/urls.py file:

```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('date', views.date, name='date')
]
```

Run the server again:

```sh
python ./manage.py runserver 127.0.0.1:9999
```

Using cURL we can test the endpoint works as expected:

```
curl http://127.0.0.1:9999/firstapp/date

{"date": "2020-05-15 22:10:22"}
```






















# Djanog App Reactive Integration

In this part of the tutorial we'll add reactive connection using a websocket using Django channels and the `djangorestframework-reactive` project.

In the python virtual environment, install the reactive pip package:

```sh
pip install djangorestframework-reactive
```

In `tutorial/settings.py`, add `rest_framework_reactive` to the `INSTALLED_APPS` list.

Create a new module 