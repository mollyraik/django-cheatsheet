# Django New Project Steps
## In TERMINAL:
1. from root directory: ```createdb <db_name>```
2. cd into ```django_env```
3. run ```pipenv shell``` or ```python3 -m pipenv shell```
4. cd back into root directory
5. run ```django-admin startproject <project_name>```
6. cd ```<project_name>```
7. open vscode ```code .```
8. to run the development server:  
    ```python manage.py runserver```

## In ```settings.py```
1. update DATABASES {} dict in settings.py
2. IN TERMINAL : setup django app for functionality using:  
        ```python manage.py startapp <app_name>```
3. add '<app_name>' to INSTALLED_APPS []
4. you can update the TIME_ZONE in 'Internationalization'
----
## In ```urls.py```
1. import ```include``` from ```django.urls```
2. within ```urlpatterns``` list add:  
        ```path('', include(<app_name>.urls)),``` <-- the empty string is the root path

----
> ## next: touch ```urls.py``` within <app_name> directory**

----

## In ```<app_name>.urls.py```
1. import path, views  
```from django.urls import path```  
```from . import views```
2. add ```urlpatterns``` list:  
```
    urlpatterns = [  
        path('', views.home),  
        path('<path_name>/', views.<path_name>)
        path('<path_name/<int:parameter_id>/', views.<parameter_path_name>)  
    ]

    # NOTE: django paths NEVER begin with a '/' -- django prepends this
```    

----
> ## ```mkdir <app_name>/templates```  

> ## ```touch <app_name>/templates/<page_name>.html```  
----
## In ```views.py```
1. define a function for each route:  
```
    def <page_name>(request):  
        return render(request, '<page_name>.html')
```
----
# Using Template Inheritance 

## ```base.html``` buildout
** add html boilerplate and repeated content **  
** put content blocks around title and main content **  
** { load static} connects any css, js, or img files you may make in a ```static``` directory **

```
{% load static %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css"
    />
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
    <!-- <script defer src="{% static 'js/script.js' %}"></script> -->
    {% block title%}
    <title>Default Title</title>
    {% endblock %}
</head>
<body>
    <header class="navbar-fixed">
       <nav>
        <div class="nav-wrapper deep-purple lighten-3">
            <a href="/" class="brand-logo left">CatCollector</a>
            <ul class="right">
                <li><a href="/about">About</a></li>
            </ul>
        </div>
       </nav> 
    </header>
    <main class="container">
        {% block content %} 
        {% endblock %}
    </main>
    <footer class="page-footer deep-purple lighten-3">
        <p>All Rights Reserved, Copyright &copy; CatCollector 2023 &nbsp;</p>
    </footer>
</body>
</html>
```

## and in content pages ...
```
{% extends 'base.html' %}

{% block title %}
<title>Specific title for the page</title>
{% endblock %}

{% block content%}
    <h1>Specific HTML for the page</h1>
    <p>This is a template</p>
{% endblock %}
```
----
# Rendering Data in Templates
1. add data as a "context dictionary" on views function:
```
def <page_name>(request):
    return render(request, '<page_name>.html', {
        'data_variable': data_variable,
    })
```
2. inside the template:
```
# double curly braces outputs data 
    {{ data.name }}

# for loops and if statements need endblocks:
    {% for item in data %}
        <p>{{ item.name }}</p>
    {% end for %}

    OR 

    {% if item.price > 0 %}
        <p>Price: {{ item.price }}</p>
    {% else %}
        <p>Price: FREE! </p>
    {% endif %}
```
----
# Defining a Model:
## in ```models.py```
```
class ModelName(models.Model):
    string_field=models.CharField(max_length=100) # <-- max_length optional
    int_field=models.IntegerField
    larger_string=models.TextField

    def __str__(self):
        return self.name # <-- this is optional to get a friendlier 
                                response from printing
```
----
# Making Migrations
## in TERMINAL:
1. run ```python manage.py migrate``` to connect database
2. ```python manage.py makemigrations``` to stage models
3. rerun ```python manage.py migrate``` to complete migration
----
# Creating an Admin Acct:
## in TERMINAL
1. run ```python manage.py createsuperuser```
2. create user log in 
3. go to ```/admin``` to log in

## Connect model to admin
### in ```admin.py```
```
from .models import ModelName

admin.site.register(ModelName)
```
Now you can perform CRUD on your data from the admin site

----
# Preferred Django Method for citing urls:
## in ```main_app.urls.py```
```
urlpatterns = [  
        path('', views.home),  
        path('<path_name>/', views.<path_name>, name='path_name')  
    ]
```
## on pages where urls are linked...
```
<a href="{% url 'path_name' any_specific_parameter %}"> </a>
```