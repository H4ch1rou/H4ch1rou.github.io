---
layout: single
title: Apuntes de Framework Django
date: 2022-08-13
classes: wide
header:
  teaser: /assets/images/masthead.png
categories:
  - Python
  - ApuntesUtiles
  - Programacion
tags:
  - H4ch1rou
  - 
  -
---

## Apuntes de Django

Django es un framework de desarrollo web basado en pyhon.

## Creacion del entorno virtual

Primero que nada se realizara la creacion de un entorno virtual (virtualenv) para no afectrar a las versiones del sistema operativo

```bash
  #creacion de entorno virtual
  python -m venv "nombre del entorno"

  source activate (dentro de la ruta del venv)
  
  #instalacion de django 
  python -m pip install django 

  #creacion del proyecto 
  python -m django startproject "nombreproyecto" 

  #ejecucion del proyecto creado
  python -m manage.py runserver 

```

## Archivo settings.py 

El archivo settings.py contiene configuraciones referentes al proyecto que son de bastante reelevancia en nuestro desarrollo, por ejemplo 

la variable SECRET_KEY, no debe en ningun caso ser compartida ya que este permitira que nuestro sea ejecutado por terceros. 

la variable DEBUG, es aquel que define que los errores sean mostrandos por pantalla a los usuarios al momento de ocurrir 

la variable ALLOWED_HOSTS es aquel que define cuales son las direcciones IP que tienen permitido ingresar a la aplicacion

## Separacion de entornos

si creas una carpeta llamada **settings** (esta debe contener un archivo __init__.py) puedes separar el archivo de configuracion en diferentes entornos(produccion, testing, local), para esto debemos cambiar el nombre del archivo settings.py creado por defecto.

Dentro de la carpeta **settings** crearemos 1 archivo base.py este contendra toda la configuracion que se debe aplicar en los diferentes entornos, y en archivos .py con el nombre de los entornos agregar la configuracion dinamica que debe existir, por ejemplo conexiones a base de datos, rutas de produccion, etc.e importar el archivo base.py para que obtenga la informacion que debe contener los ambientes. 


en el archivo manage.py indicaremos donde esta la configuracion que se debe ejecutar por defecto 

El primero indica que se debe ejecutar el archivo clase2.settings.py (archivo creado por defecto)

El segundo indica cual es el archivo del ambiente que se debe ejecutar.

En el caso de que quisieramos hacerlo de forma dinamica al momento de ejecutar el manage.py runserver debemos hacerlo de la siguiente forma 

**python3 manage.py runserver --settings=clase2.settings.local**

```bash 

  #archivo manage.py
  #original
  os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'clase2.settings') 
  #modificado
  os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'clase2.settings.local') 
```

## Aplicaciones en django

https://docs.djangoproject.com/en/4.1/ref/applications/

Este sirve para separar tu proyecto en pequeñas aplicaciones que pueden ser reutiliadas en diferentes proyectos de la empresa. por lo tanto es ideal lograr 
que las aplicaciones que crees sean independientes. 

Para generar un orden en las aplicacion django se puede crear una carpeta a la altura del manage.py que se llame applications, este con la finalidad de generar un orden en la aplicacion en el caso de que se creen muchas aplicaciones. 


```bash
  #creacion de aplicacion dentro de nuestro proyecto
  python3 manage.py startapp "nombre"

```

Posterior a la creacion de las aplicacion estas deben ser agregadas a nuestro proyecto, para eso debemos modificar el archivo base.py 

```bash
## Ejemplo 
| INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    #local apps
    'applications.departamento' #CARPETA.ARCHIVO
]
```

y luego de esto modificar el nombre de la ruta del la apps en el archivo APP.py dentro de la carpeta de la app creada

```bash
  class EmpleadosConfig(AppConfig):
      default_auto_field = 'django.db.models.BigAutoField'
      name = 'applications.empleados'
```

## Patron de diseño Modelo vista Template 


el patron de diseño MVT separa el programa en 3 secciones 

- Modelo: corresponde a la informacion de la conexion con la base de datos
- Vista: corresponde a la logica del negocio (controlador en el modelo MVC)
- Template: la vista que se le mostrara al usuario usualmente HTML 


## Templates 

Se debe crear una carpeta a la altura de manage.py que se llame template, esta tendra los archivos front (html,js,etc) de nuestro proyecto. 

para que este pueda encontrarlos de forma nativa se requiere modificar el valor del settings.py 

```bash

BASE_DIR = Path(__file__).resolve().parent.parent.parent

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'], # aqui se agregan las rutas de los archivos template
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]<>
```

## Creacion de los Models 

Los modelos son las representaciones de las bases de datos, pero aqui nos olvidamos de la creacion de consultas SQL si no que usamos propio lenguaje python ORM para poder gestionar la base de datos.

Una vez que creamos las clases en el model 

```bash 
  # revisa la informacion que se a actualizar 
  python3 manage.py makemigrations

  # realiza la migracion a la base de datos
  python3 manage.py migrate

```

# Archivo admin.py 

es un archivo que permite gestionar el modelo de base de datos a traves de una plataforma web. 

para crear el usuario de la base de datos se debe ejecutar por consola 

```bash
  #creacion del usuario de administracion
  python manage.py createsuperuser

```






