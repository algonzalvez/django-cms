![N|Solid](http://docs.django-cms.org/en/latest/_images/django-cms-logo.png)
##
##
# Instalación y configuración de Django-cms
 
Este manual explica los pasos de instalación y configuración de Django-cms en Ubuntu server 18.04.3. Para ello se instalará postgreSQL como base de datos.

## Requisitos previos a la instalación de DJANGO-CMS
### Actualización e instalación de python-dev y postgresql

  - Actualizar los repositorios e instalar actualizaciones.
```sh
$ sudo apt update && sudo apt upgrade -y
```
  - Instalar Python-dev, y postgreSQL con sus complementos.
```sh
$ sudo apt-get install python-dev libpq-dev postgresql postgresql-contrib -y
```
### Creación de base de datos y usuario de base de datos postgreSQL.
- Poner contraseña al usuario postgres.
- Iniciamos como usuario postgres y a continuación abrimos consola psql
```sh
$ sudo passwd postgres
$ su postgres
$ psql
```
- Creamos la base de datos y el usuario. (para salir de psql: \q)
```sh
CREATE DATABASE djangoprojectdb;
CREATE USER djangoproject WITH PASSWORD 'djangoproject';
ALTER ROLE djangoproject SET client_encoding TO 'utf8';
ALTER ROLE djangoproject SET default_transaction_isolation TO 'read committed';
ALTER ROLE djangoproject SET timezone TO 'EST';
GRANT ALL PRIVILEGES ON DATABASE djangoprojectdb TO djangoproject;
\q
```
- Salir del usuario postgres
```sh
$ exit
```
### Instalar el sistema de gestión de paquetes de python pip
```sh
$ sudo apt install python-pip -y
```
### Instalar la herramienta para crear entornos aislados para python
```sh
$ sudo apt install virtualenv -y
```
### Crear y activar un entorno virtual
```sh
$ virtualenv env
$ source env/bin/activate
```
## Instalación de DJANGO-CMS
### Instalación con pip de DJANGO-CMS
```sh
$ pip install  django-cms-installer
```
### Creación del proyecto
```sh
$ djangocms a128
```
- Cambiamos al directorio raíz de nuestro proyecto
```sh
$ cd a128
```
### Configuración de Django-cms
- Editar el fichero de opciones.
```sh
vim a128/settings.py
```
- Cambiar la línea 'ALLOWED_HOSTS = [ ]'. Introducir la ip del servidor.
```sh
ALLOWED_HOST = ['192.168.128.180']
```
- Modificar el campo DATABASES para que quede asi:
```sh
    DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'djangoprojectdb',
        'USER': 'djangoproject',
        'PASSWORD': 'djangoproject',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```
Guardar y cerrar el archivo settings.py

### Instalar la librería que permite conectar Django-cms con postgreSQL
```sh
$ pip install psycopg2==2.7.7
```
### Migrar la base de datos de postgreSQL a Django-cms
```sh
$ python manage.py migrate
```
### Crear el usuario administrador de Django-cms
```sh
$ python manage.py createsuperuser
```
### Iniciar Django-cms
```sh
$ python manage.py runserver 0.0.0.0:8000
```
___
### Instalación de Addons
En la página web de [marketplace] tenemos a nuestra disposición una gran variedad de addons o plugins que amplian la funcionalidad de nuestro sitio web. En esta ocación decidimos instalar el addon de audio.

#### Instalar django-filer 
**La instalación de django-file solo hay que realizarla si es la primera vez que instalamos un addon en nuestro proyecto.**
```sh
pip install django-filer
```
Editar de nuevo el archivo de configuración settings.py

Agregar al apartado de INSTALLED_APPS las siguientes líneas:
```sh
'filer',
'mptt',
'easy_thumbnails',
'djangocms_audio',
```
Guardar y salir del archivo settings.py
#### Instalar el addon de audio
```
pip install djangocms-audio
```

#### Migrar la nueva configuración
Situarse en el directorio raíz del proyecto y ejecutar:
```sh
python manage.py migrate
```
#### Iniciar Django-cms
```sh
$ python manage.py runserver 0.0.0.0:8000
```
Ahora aparecerá la opción de agregar el Audio Player a nuestro sitio web.
##
# ¡Muchas gracias a todos!

###### Este manual ha sido elaborado por:
- Francisco M. Cadena
- Ángel L. Gonzálvez
- Alejandro M. Vázquez

[//]: # 
   [marketplace]: <https://marketplace.django-cms.org/>
