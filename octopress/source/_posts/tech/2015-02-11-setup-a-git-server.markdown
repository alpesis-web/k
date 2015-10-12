---
layout: post_tech
title: "Setup A Git Server"
date: 2015-02-11 02:00:14 +0800
comments: true
categories: [tech]
tags: [git, web]
---

## Prequistions

- Ubuntu 12.04
- Git Server/Client
- Gitolite
- Gitweb
- Apache

## 1. Git Server

installation

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install git-core
```

configuration

```bash
$ sudo adduser --system --shell /bin/bash --group --disabled-password --home /home/git git
$ sudo passwd git
```

## 2. Gitolite

installation

```bash
$ sudo apt-get install gitolite

$ cp ~/.ssh/id_rsa.pub /tmp/$(whoami).pub
$ sudo su - git
$ gl-setup /tmp/*.pub

# config the path of gitolite
# save and exit
# path: /home/git/project.list
# path: /home/gt/repositories

$ exit
```

configuration

```bash
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"

$ mkdir gitadmin
$ sudo chown -R user:group /gitadmin

$ cd /gitadmin
$ git clone git@ip:gitolite-admin.git
$ cd gitolite-admin

# config key and gitolite.conf

$ git add .
$ git commit -m "comment"
$ git push origin master
```


## 3. Gitweb

installation

```bash
$ sudo apt-get install gitweb
```

configuration

```bash
$ cd /var/www/
$ sudo mkdir gitweb
$ cd gitweb
$ sudo ln -s /usr/share/gitweb/* .  # note: . is a must

$ sudo nano /etc/gitweb.conf
$ sudo chmod 777 -R /home/git/repositories
```

gitweb.conf

```
# path to git projects (<project>.git)
#$projectroot = "/var/cache/git";
$projectroot = "/home/git/repositories";

# directory to use for temp files
$git_temp = "/tmp";

# target of the home link on top of all pages
#$home_link = $my_uri || "/";

# html text to include at home page
#$home_text = "indextext.html";

# file with project list; by default, simply scan the projectroot dir.
#$projects_list = $projectroot;
$projects_list = "/home/git/project.list";

# stylesheet to use
@stylesheets = ("http://202.120.83.104:8090/static/gitweb.css");

# javascript code for gitweb
$javascript = "http://202.120.83.104:8090/static/gitweb.js";

# logo to use
$logo = "http://202.120.83.104:8090/static/git-logo.png";

# the 'favicon'
$favicon = "http://202.120.83.104:8090/static/git-favicon.png";

# git-diff-tree(1) options to use for generated patches
#@diff_opts = ("-M");
@diff_opts = ();
```


## 4. Apache

installation

```bash
$ sudo apt-get install apache2
```

configuration

```bash
# note1: web path - /var/www
# note2: cgi path - /usr/lib/cgi-bin/
$ cd /usr/lib/cgi-bin/               # check if gitweb.cgi is here

$ sudo nano /etc/apache2/sites-available/default
$ sudo nano /etc/apache2/ports.conf
$ sudo nano /etc/apache2/apache2.conf
$ sudo /etc/init.d/apache2 restart
```

/etc/apache2/sites0available/default

```
<VirtualHost *:8090>
        #ServerAdmin webmaster@localhost
        ServerAdmin insight@202.120.83.104

        DocumentRoot /var/www/gitweb
        <Directory />
                Options FollowSymLinks
                AllowOverride None
        </Directory>
        <Directory /var/www/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
        </Directory>

        SetEnv GIT_PROJECT_ROOT /home/git/repositories
        SetEnv GIT_HTTP_EXPORT_ALL
        ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
        <Directory "/usr/lib/cgi-bin">
                AllowOverride None
                Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
                Order allow,deny
                Allow from all
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log

        # Possible values include: debug, info, notice, warn, error, crit,
        # alert, emerg.
        LogLevel warn

        CustomLog ${APACHE_LOG_DIR}/access.log combined

    Alias /doc/ "/usr/share/doc/" 
    <Directory "/usr/share/doc/">
        Options Indexes MultiViews FollowSymLinks
        AllowOverride None
        Order deny,allow
        Deny from all
        Allow from 127.0.0.0/255.0.0.0 ::1/128
    </Directory>

</VirtualHost>
```

/etc/apache2/ports.conf

```
NameVirtualHost *:8090
#Listen 80
Listen 202.120.83.104:8090
```

/etc/apache2/apache2.conf

```
# Gitweb host
ServerName 202.120.83.104:8090
```

## Testing


link: http://ip_address/gitweb
