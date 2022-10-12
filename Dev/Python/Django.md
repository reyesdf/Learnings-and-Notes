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

## Creating admin credentials for django admin page

- `python3 manage.py createsuperuser`
- enter details like username, email and password
- if password is too short, you can have the option to bypass it
- access admin page via: `http://127.0.0.1:8000/admin/` and enter created credentials

## Creating models for the database

example code from models.py (polls app):

```python
class Question(models.Model):
  question_text = models.CharField(max_length=200)
  pub_date = models.DateTimeField('date published')

  def __str__(self):
    return self.question_text

class Choice(models.Model):
  question = models.ForeignKey(Question, on_delete=models.CASCADE) #when question is deleted, all the choices of that question is also deleted
  choice_text = models.CharField(max_length=200)
  votes = models.IntegerField(default=0)

  def __str__(self):
    return self.choice_text
```

## Registering those models

example code from admin.py (polls app):

```python
from django.contrib import admin

# Register your models here.
from .models import Question, Choice


class ChoiceInline(admin.TabularInline):
  model = Choice
  extra = 3

class QuestionAdmin(admin.ModelAdmin):
  fieldsets = [
    (None, {'fields': {'question_text'}}),
    ('Date Information', {'fields': ['pub_date'], 'classes': ['collapse']}),
  ]

admin.site.register(Question,QuestionAdmin)
```

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

## Creating a global templates folder

- create a folder in the root on the project, and name it templates for example.
- add the other folders for the app, example: "polls"
- add index.html

- expose the path of the templates
  - in settings.py (pollster folder)
    - import os, and add the following line of code to the Templates list, on DIR: `[os.path.join(BASE_DIR, 'templates')]

### Create a base html

- in the root templates folder, create base html file `base.html`

add html boilerplate with bootstrap for styling:

```html
<!DOCTYPE hmtl>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- CSS only -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi"
      crossorigin="anonymous"
    />
    <title>Pollster {% block title %}{% endblock %}</title>
  </head>
  <body>
    <div class="container">
      <div class="row">
        <div class="col-md-6 m-auto">{% block content %}{% endblock %}</div>
      </div>
    </div>
  </body>
</html>
```

then extend it in the index.html (templates/polls/index.html)

```python
{% extends 'base.html' %} {% block content %} Polls {% endblock %}
```
