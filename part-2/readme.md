# Starting Our First App

If you haven't yet - start at part-1 and get django up and running, connected to postgres for storage.

In order to create out first app we again use the `manage.py` tool from within the python virtual environment:

```sh
$ python ./manage.py startapp firstapp
```

That gets us an app under the tutorial directory which has some python files in that relate to a django app:

```sh
(env) [bjs@localhost tutorial]$ tree firstapp/
firstapp/
├── admin.py
├── apps.py
├── __init__.py
├── migrations
│   └── __init__.py
├── models.py
├── tests.py
└── views.py

1 directory, 7 files
```


We need something to display and a target url for the app to live.

**firstapp/views.py**

```python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
    return HttpResponse("<h1>First App</h1>")
```

Create a new file:

**firstapp/urls.py**

```python
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

The URLs configured in the application are application-relative. So we don't show the full URL (These should be URIs) here.

Now we need to point the main project to the application so it is aware of where to serve the application.

We're making a simple public facing application here.

It's a one-line fix to `tutorial/urls.py`, just add the following into the urlpatterns list:

```python
    path('polls/', include('polls.urls')),
```

The whole of `tutorial/urls.py` should look like:

```python
from django.contrib import admin
from django.urls import include,path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('firstapp/', include('firstapp.urls')),
]
```

That should tie the application to `/firstapp/`

Again, start the django server 

```sh
python ./manage.py runserver 127.0.0.1:9999
```

Now, back to the browser and surf to the application URL: http://127.0.0.1:9999/first/app

![](/images/part-2-firstapp.png)

Not particularly exciting, but hey - at least it we have a view going in our first application.

