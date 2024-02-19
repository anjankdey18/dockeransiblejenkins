# django_ci_cd

##for everybody Youtube video link: https://www.youtube.com/watch?v=Sl94H5e7MPw

## A CI CD Project based on Jenkins Tool for Django Project (Python)
This is a Django project that runs a single page web application. It allows one to eliminate the admin overhead that comes with testing your Django project in a production like environment. This will allow you to make changes and see the effects almost immediately.

## Create virtual env for Django Project (Python)
in your project dir, create virtual environment named "env" using the 
command: python3 -m venv env 
and you will be able to see env dir within your project. then activate your env using the 
command: source env/bin/activate 
when it is activate successfully you will be able to see 
[ans@centos9ansmaster django_cicd]$ source env/bin/activate
(env) [ans@centos9ansmaster django_cicd]$ 

Installing an official release with pip¶
This is the recommended way to install Django.

1. Install pip. The easiest is to use the standalone pip installer. If your distribution already has pip installed, you might need to update it if it’s outdated. If it’s outdated, you’ll know because installation won’t work.

2. Take a look at venv. This tool provides isolated Python environments, which are more practical than installing packages systemwide. It also allows installing packages without administrator privileges. The contributing tutorial walks through how to create a virtual environment.

3. After you’ve created and activated a virtual environment, enter the command:

$ python -m pip install Django

Then check the Django version using the command: pip freeze
(env) [ans@centos9ansmaster django_cicd]$ pip freeze
asgiref==3.7.2
Django==4.2.10
sqlparse==0.4.4
typing_extensions==4.9.0

4. start the sample django project named "app" using the command: django-admin startproject app
(env) [ans@centos9ansmaster django_cicd]$ django-admin startproject app

**1)** If you follow the youtube video then it means you'll be using Jenkins to set up a CI/CD pipeline. Now this project is specific to Django project but you can edit the files to fit your use case scenario. I'll be explaining what each file used in the pipeline does and variables you'll need to change for your project to run. Again this is specific for Django projects. 

**2)** `initial-setup.sh` - This file is the first file to look at when setting up this project. It installs the required packages to make this project work such as Nginx, Jenkins, Python etc. Refer to the youtube video to see how and when it is used.

**3)** `Jenkinsfile` - This file contains the definition of the stages in the pipeline. The stages in this project's pipeline are `Setup Python Virtual Environment`, `Setup gunicorn service` and `Setup Nginx`. The stages in this pipeline just does two things. First it makes a file executable and then runs the file. The file carries out the commands that is described by the stage description.

**4)** `envsetup.sh` - This file sets up the python virtual environment, installs the python packages and then creates log files that will be used by Nginx. 

**5)** `gunicorn.sh` - This file runs some Django management commands like migration commands and static files collection commands. It also sets up the gunicorn service that will be running the gunicorn server in the background.

**6)** `nginx.sh` - This file sets up Nginx with a configuration file that points Nginx to the gunicorn service that is running our application. This allows Nginx serve our application. I have followed a digital ocean article to setup this file. You can go through the video once to replicate sites-available and sites-enabled scanerio. 

**7)** `app.conf` - This is an Nginx server configuration file. This file is used to setup Nginx as proxy server to gunicorn. For this configuration to work, change the value of `server_name` to the IP address or domain name of your server. 

**8)** `gunicorn.service` - This is a Systemd Service configuration file. This configures the gunicorn server to run in the background as a service. It also sets the path to the gunicorn sock file. At the end of the `ExecStart` value, change it to the path of the `wsgi` file of the Django Project. Also set the User to the one that owns the project folder according to the configuration from the youtube video. 

**9)** The rest of the files are normal files you find in a Django Project. Please if you add ssl to your domain then you'll have to update the `app.conf` with the configuration of your ssl.

## :page_with_curl:  _Instructions For Running This Project Locally_

**1)** Fire up your favourite console & clone this repo somewhere:

__`❍ git clonehttps://github.com/ishwar6/django_ci_cd.git`__

**2)** Enter this directory:

__`❍ cd django_ci_cd`__

**3)** Start the server to view the webapp:

__`❍ python manage.py runserver `__

**4)** Open your browser and type in this URL to view the webapp:

__`❍ http://127.0.0.1:8000/`__

__*Happy developing!*__
