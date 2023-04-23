# CRUD Api in Django 
* Simple App used as BackEnd for nextJs13-firsapp 
* Django Rest Framework will be used.

## REQUIREMENTS
- python v.3.10
- pip v.22.2.1
- pipenv v.2022.7.24
- Django v.4.2 will be used (support Abril 2026)

## INSTALLATION

### Django
```
mkdir tasks_api
cd tasks_api
pipenv shell    # create virtual enviroment
pipenv install django
```

### Create Project
```
django-admin startproject core .    # ends with a dot
```

### Install Packages
```
pipenv install djangorestframework
pipenv install django-cors-headers
```

### Show Installed packages
```
pip list

Package             Version
------------------- -------
asgiref             3.6.0
Django              4.2
django-cors-headers 3.14.0
djangorestframework 3.14.0
pip                 22.3.1
pytz                2023.3
setuptools          65.7.0
sqlparse            0.4.4
wheel               0.38.4
```

### Directory tasks_api
```
tasks_api/
    db.sqlite3
    manage.py
    Pipfile
    Pipfile.lock
    readme.md
    core/
        __init__.py
        asgi.py
        settings.py
        urls.py
        wsg1.py
```

### Settings ( core/settings.py )

1. MOdify INSTALLED_APPS:
```
# Rename INSTALLED_APPS for DJANGO_APPS
DJANGO_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# Add PROJECT_APPS 
PROJECT_APPS = [
]

# Add THIRD_PARTY_APPS
THIRD_PARTY_APPS = [
    'corsheaders',
    'rest_framework',
]

# Join variables
INSTALLED_APPS = DJANGO_APPS + PROJECT_APPS + THIRD_PARTY_APPS
```

2. Add into MIDDLEWARE (before: 'django.middleware.common.CommonMiddleware')
```
MIDDLEWARE = [
	...
	'corsheaders.middleware.CorsMiddleware',
    ...
]
```

3. Add variable CORS_ALLOWED_ORIGINS
```
    # Cors authorization
    CORS_ALLOWED_ORIGINS = [
    
] 
```

### Test app
```
python manage.py runserver  # test localhost:8000
```

### Git (main)
#### Create .gitignore
Generate from https://www.toptal.com/developers/gitignore

```
git init
...
git push ... origin main
```


## DEVELOP

### Optional: Create branch develop
```
git checkout -b develop
```

### Select interpreter Python en VsCode
1. VsCode Extension must be installed
2. To know the name of the virtual environment
```
pipenv --venv
```
3. Select virtual enviroment created
- ctrl + shift + p  select interpreter

### Create App
```
python manage.py startapp tasks
```

### Update core/settings.py
```
 PROJECT_APPS = [
    'tasks'    
]
```

### Migrate 
```
python manage.py migrate
```

### Create Model Task
tasks/models.py
```
class Task(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField(blank=True)
    done = models.BooleanField(default=False)

    def __str__(self):
        return self.title
```

#### Makemigrations
```
python manage.py makemigrations tasks

python manage.py migrate tasks
```

### Create super user
```
python manage.py createsuperuser
```

### Creating Api
1. Create serializer
- new file tasks/serializer.py 
```
from rest_framework import serializers
from .models import Task

class TaskSerializer(serializers.ModelSerializer):
    class Meta:
        model = Task
        fields = ('id', 'title', 'description', 'done')
        #fields = '__all__'
```

2. Create view
- update file task/views.py
```
from rest_framework import viewsets
from .models import Task
from .serializer import TaskSerializer

# Create your views here.
class TaskView(viewsets.ModelViewSet):
    serializer_class = TaskSerializer
    queryset = Task.objects.all()
```

2.1 Note:
The ModelViewSet class automatically implements HTTP methods GET, POST, PUT, PATCH, and DELETE for model-based views. Additionally, it provides a set of default actions that can be customized or extended as needed.

3. Create routes for app tasks
- create file tasks/urls.py
```
from django.urls import path, include
from rest_framework import routers
from tasks import views

router = routers.DefaultRouter()
router.register(r'tasks', views.TaskView, 'tasks')

urlpatterns = [
    path('api/v1/', include(router.urls))
]
```

4. Add routes created to core
- Update urlpatterns in core/urls.py
```
urlpatterns = [
    ...
    path('tasks/', include('tasks.urls')),
]
```

5. Test api 
- url base: localhost:8000/tasks/api/v1/tasks
- test from navigator or with Thunder Client extension in VSCode
- GET: localhost:8000/tasks/api/v1/tasks
- GET <id>: localhost:8000/tasks/api/v1/tasks/3
- POST : localhost:8000/tasks/api/v1/tasks/
- PUT : localhost:8000/tasks/api/v1/tasks/<id>/
- DELETE: localhost:8000/tasks/api/v1/tasks/<id>/
