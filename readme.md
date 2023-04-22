# CRUD Api in Django 
Used as BackEnd for nextJs13-firsapp 

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
]
```

3. Add variable CORS_ALLOWED_ORIGINS
```
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
git checkout -b develop

