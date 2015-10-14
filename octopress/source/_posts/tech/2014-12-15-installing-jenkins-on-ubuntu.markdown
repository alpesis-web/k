---
layout: post_tech
titlde: "Installing Jenkins on Ubuntu"
date:   2014-12-15 11:46:00 +0800
comments: true
categories: [tech]
tags: [jenkins, ubuntu]
---

## Jenkins

installation

```bash
$ wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
$ sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
$ sudo apt-get update
$ sudo apt-get install jenkins
```

open the link: http://202.120.83.101:8080

## Nginx


installation

```bash
$ sudo aptitude -y install nginx

# remove default setting
$ cd /etc/nginx/sites-available
$ sudo cp default default.backup
$ sudo rm default ../sites-enabled/default
```

/etc.nginx.sites-available: add the configurations of jenkins

```bash
$ sudo nano jenkins

upstream app_server {
server 127.0.0.1:8080 fail_timeout=0;
}

server {
listen 80;
listen [::]:80 default ipv6only=on;
server_name ci.yourcompany.com;

location / {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;

    if (!-f $request_filename) {
	proxy_pass http://app_server;
	break;
    }
}
}
```

adding the link

```bash
$ sudo ln -s /etc/nginx/sites-available/jenkins /etc/nginx/sites-enabled/
```

start service

```bash
$ sudo service nginx restart
```

## Configurating Jenkins

installing java

```bash
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java7-installer
$ sudo update-java-alternatives -s java-7-oracle  
$ sudo update-alternatives --config java
```

configurating java home

```bash
$ sudo nano $HOME/.bashrc
```

adding the configuration

```bash
# set JAVA_HOME
JAVA_HOME=/usr/lib/jvm/java-7-oracle/ 
export JAVA_HOME
PATH=$PATH:$JAVA_HOME
export PATH
```

source

```bash
source $HOME/.bashrc
echo $JAVA_HOME
```

configurating jenkins

- Manage Jenkins -> Configure System -> JDK/Ant/Maven

- Manage Jenkins -> Configure Global Security -> security/matrix/admin

- restart jenkins, sign up with admin

- Manage Jenkins -> Manage Plugins -> Git Plugin
