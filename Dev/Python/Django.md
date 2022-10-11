# Django

1. mkdir <project_name> and cd to that folder -> directory of project
2. install python if not yet installed
3. install pipenv (virtual env) `pip3 install pipenv`
4. install django `pipenv install django`
5. start virtual environment `pipenv shell`
6. start a project: `django-admin startproject <project_name> .` -> core directory of project
   - the period will tell django to not create another directory.


## Using Integrated Terminal

1. pipenv --venv
2. copy path
3. ctrl + shift + P -> python:interpreter -> add interpreter
4. add path of venv and append /bin/python at end of path
5. you are now using the venv of that project :rocket:

## Creating app inside project

- on new terminal `python3 manage.py startapp <app_name>`
- everytime you create an app, make sure to add it to the settings of the root app.

## Writing a View / Request Handler

in views.py:

```python
from django.shortcuts import render
from django.http import HttpResponse
# Create your views here.
#takes a request and response (request handler)


def say_hello(request):
  return HttpResponse('Hello World')
```

then create a `urls.py` in that app folder and add this code:

```python
from django.urls import path
from . import views

# URL Configuration 
urlpatterns = [
  #views.say_hello is from views.py, it is calling a reference function
  path('hello/' , views.say_hello) 
]
```

this will be the URL Config for the "hello/" path.
after creating the URL Config, add the config to `urls.py` in the root project:

```python
rom django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('playground/', include('playground.urls'))
]
```

here we add the `include` in import path, and added the playground url in the URL Config in the urls.py for the root project.
