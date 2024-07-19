django-testing
--------------
followed: https://dockerize.io/guides/python-django-guide

## initialize env
```sh
python3 -m venv env && source env/bin/activate
source env/bin/pip install -r requirements.txt
```

## start project/app

```sh
django-admin startproject <mysite>
cd <mysite>
# this will run django onto your localhost:8000 <- can go there in browser
python3 manage.py runserver

# create an app
python3 manage.py startapp <hello_world>
```


## hello world example
```python
# add "hello_world", to mysite/settings.py
# then add the following to hello_world/views.py:
def hello_world(request):
    return render(request, 'hello_world.html', {})
```
```sh
# make templates dir in hello_world
echo '<h1>Hello, World!</h1>' | tee hello_world/templates/hello_world.html
```
```python
# add these lines to mysite/urls.py
from django.contrib import admin
from django.urls import path, include        # <-- ADD THIS LINE

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('hello_world.urls')),   # <--  ADD THIS LINE
]
```

```python
# add these lines to hello_world/urls.py
from django.urls import path
from hello_world import views

urlpatterns = [
    path('', views.hello_world, name='hello_world'),
]
```


Now, when you run `python3 manage.py runserver` you should see
Hello, World! in localhost:8000

## Containerize it
```sh
podman build -t python-django-app .
run -it -p 8000:8000 python-django-app
```
