---
layout: post_tech
title:  "Configurating Nginx for Django Projects on Ubuntu"
date:   2015-02-01 10:54:00 +0800
comments: true
categories: [tech]
tags: [nginx, ubuntu, linux, supervisor, gunicorn, django]
---

## 1. Nginx

installation

```bash
$ sudo apt-get install nginx
```

configurating users

```bash
$ sudo groupadd --system edx
$ sudo useradd --system --gid edx --home /edx/analytics/apps/edx-analytics-data-api analyticsapi
$ sudo useradd --system --gid edx --home /edx/analytics/apps/edx-analytics-dashboard analyticsdashboard
    
$ sudo chown -R insight:insight /edx/analytics/apps/edx-analytics-data-api
$ sudo chmod -R g+w /edx/analytics/apps/edx-analytics-data-api
    
$ sudo chown -R insight:insight /edx/analytics/apps/edx-analytics-dashboard
$ sudo chmod -R g+w /edx/analytics/apps/edx-analytics-dashboard
```

## 2. Supervisor

installation

```bash
$ sudo apt-get install supervisor
```

configurating supervisor

```bash
$ cd /edx/analytics
$ mkdir -p var/logs
$ cd var/logs
$ touch gunicorn_supervisor.log 
```

## 3. Django Project

django settings

```bash
$ cd /edx/analytics/apps/edx-analytics-data-api/app/analyticsdataserver/settings
$ nano production.py
```

gunicorn

```bash
$ cd /edx/analytics/apps/edx-analytics-data-api
$ source api_env/bin/activate
$ cd app
$ pip install gunicorn
$ pip install setproctitle
    
$ cd app
$ gunicorn analyticsdataserver.wsgi:application --bind 0.0.0.0:9001
    
$ cd /edx/analytics/apps/edx-analytics-data-api/api_env/bin
$ sudo nano gunicorn_start
$ sudo chown insight:insight gunicorn_start
$ sudo chmod u+x gunicorn_start
```

gunicorn_start

```bash
#!/bin/bash
NAME="analyticsapi" # Name of the application
DJANGODIR=/edx/analytics/apps/edx-analytics-data-api/ # Django project directory
SOCKFILE=/edx/analytics/apps/edx-analytics-data-api/var/run/gunicorn.sock # we will communicte using this unix socket
USER=insight # the user to run as
GROUP=edx # the group to run as
NUM_WORKERS=3 # how many worker processes should Gunicorn spawn
DJANGO_SETTINGS_MODULE=analyticsdataserver.settings # which settings file should Django use

echo "Starting $NAME" 

# Activate the virtual environment
cd $DJANGODIR
source api_env/bin/activate
#export DJANGO_SETTINGS_MODULE=$DJANGO_SETTINGS_MODULE
export PYTHONPATH=/edx/analytics/apps/edx-analytics-data-api/api_env/bin/python

# Create the run directory if it doesn't exist
RUNDIR=$(dirname $SOCKFILE)
test -d $RUNDIR || mkdir -p $RUNDIR

# Start your Django Unicorn
# Programs meant to be run under supervisor should not daemonize themselves (do not use --daemon)
cd app
gunicorn \
--name $NAME \
--workers $NUM_WORKERS  \
analyticsdataserver.wsgi:application --bind 0.0.0.0:9001
```

supervisor

```bash
$ sudo nano /etc/supervisor/conf.d/analyticsapi.conf
```

analyticsapi.conf

```bash
[program:analyticsapi]
command = /edx/analytics/apps/edx-analytics-data-api/api_env/bin/gunicorn_start                    ; Command to start app
user = insight                                                                                     ; User to run as
stdout_logfile = /edx/analytics/apps/edx-analytics-data-api/var/logs/gunicorn_supervisor.log       ; Where to write log messages
redirect_stderr = true                                                                             ; Save stderr in the same log
```

run

```bash
$ sudo supervisorctl reread
$ sudo supervisorctl update
$ sudo supervisorctl start analyticsapi
```

## 4. Configurating Nginx

modification

```bash
$ sudo nano /etc/nginx/sites-available/analyticsapi
```

analyticsapi

```bash
server {
listen   8088;
server_name 202.120.83.105;

client_max_body_size 4G;

access_log /edx/analytics/apps/edx-analytics-data-api/var/logs/nginx-access.log;
error_log /edx/analytics/apps/edx-analytics-data-api/var/logs/nginx-error.log;

location ~ ^/static/ {
    autoindex on;
    expires 1y;
    root /edx/analytics/apps/edx-analytics-data-api/app/;
    #alias  /edx/analytics/apps/edx-analytics-data-api/app/assets;
    #alias http://202.120.83.105/static/rest_framework_swagger;
}

location ~ ^/media/ {         
    root /edx/analytics/apps/edx-analytics-data-api/app/; 
    expires 24h;
    access_log   off;
}

location / {
    try_files $uri @proxy;
}

location @proxy {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_pass http://0.0.0.0:9001;
}
} 
```

run

```bash
$ sudo ln -s /etc/nginx/sites-available/analyticsapi /etc/nginx/sites-enabled/analyticsapi
$ sudo service nginx restart
```

### 5. Testing

open the link: http://202.120.83.105:8088
