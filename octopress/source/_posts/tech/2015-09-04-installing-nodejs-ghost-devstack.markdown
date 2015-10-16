---
layout: post_tech
title: "Installing NodeJS Ghost Devstack"
date: 2015-09-04 20:42:51 +0800
comments: true
categories: [tech]
tags: [nodejs, ghost, git, github, mysql, nginx, forever]
---

## Prequistions

- Ubuntu 12.04/14.04
- Vagrant / VirtualBox
- Node.js / NPM
- NPM: forever
- Ghost
- MySQL
- Nginx
- Git

## 1. Vagrant / VirtualBox

```bash
$ cd /path/to/project/
$ mkdir private
$ cd private
$ vagrant init hashicorp/precise64
```

config Vagrantfile

```ruby Vagrantfile
diary_mount_dir = 'diary'

if ENV['VAGRANT_MOUNT_BASE']

  diary_mount_dir = ENV['VAGRANT_MOUNT_BASE'] + '/' + diary_mount_dir

end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 2346, host: 2346

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10" 

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "#{diary_mount_dir}", "/private/apps/diary/diary", create: true, nfs: true
```

vagrant up

```bash
$ vagrant up

$ vagrant ssh

$ sudo nano /etc/apt/sources.list  # change us to cn
$ sudo apt-get update
```

## 2. MySQL

```bash
$ sudo apt-get install mysql-server mysql-client

$ mysql -uroot 

mysql> CREATE DATABASE ghost;  
mysql> CREATE DATABASE ghost_dev;
mysql> show schemas;
mysql> exit;
```

## 3. Node.js / NPM

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo reboot

$ sudo apt-get install --yes build-essential
$ sudo apt-get install curl
$ curl --silent --location https://deb.nodesource.com/setup_0.12 | sudo bash -
$ sudo apt-get install --yes nodejs
$ node -v
$ npm -v
```

## 4. Ghost

### 4.1. Installation

```bash
$ cd /tmp
$ curl -L https://ghost.org/zip/ghost-latest.zip -o ghost.zip
$ sudo apt-get install unzip
$ sudo unzip -uo ghost.zip -d /private/apps/diary/diary/

$ cd /private/apps/diary/diary
$ npm install --production
$ npm install --development
```

### 4.2. Configuration

```bash
$ cd /private/apps/diary/diary
$ sudo nano config.js                 # config mysql db for development and production

$ npm start --development
$ npm start --production
```

config.js - development

```javascript
        //database: {
        //    client: 'sqlite3',
        //    connection: {
        //        filename: path.join(__dirname, '/content/data/ghost-dev.db')
        //    },
        //    debug: false
        //},

        database: {
              client: 'mysql',
              connection: {
                    host     : '127.0.0.1',
                    user     : 'root',
                    password : '',
                    database : 'ghost_dev',
                    charset  : 'utf8'
              },
              debug: false
        },
```

config.js - production

```javascript
        //database: {
        //    client: 'sqlite3',
        //    connection: {
        //        filename: path.join(__dirname, '/content/data/ghost-dev.db')
        //    },
        //    debug: false
        //},

        database: {
              client: 'mysql',
              connection: {
                    host     : '127.0.0.1',
                    user     : 'root',
                    password : '',
                    database : 'ghost',
                    charset  : 'utf8'
              },
              debug: false
        },
```

## 6. Forever

```bash
$ sudo npm install -g forever

$ node index.js
$ forever start index.js

$ # forever stop 0  
```

## 7. Nginx

```bash
$ sudo apt-get install nginx

$ cd /private/apps/diary
$ sudo mkdir nginx
$ cd nginx
$ sudo nano diary.conf
```

ghost.conf

```bash
server {

  listen 8000;
  server_name ghost.com;

  location / {
    proxy_set_header   X-Real-IP $remote_addr;
    proxy_set_header   Host      $http_host;
    proxy_pass         http://127.0.0.1:2368;
  }

}
```

config nginx

```bash
$ sudo ln diary.conf /etc/nginx/sites-available/diary.conf
$ ls /etc/nginx/sites-available/
$ sudo ln /etc/nginx/sites-available/diary.conf /etc/nginx/sites-enabled/diary.conf
$ ls /etc/nginx/sites-enabled/

$ sudo service nginx restart
```

## 8. Testing

- localhost:8000
- localhost:8000/admin

## 9. Auto Service Start

```bash
$ cd /private/apps/diary
$ sudo mkdir bin
$ cd bin

$ sudo nano ghost  # add the config file

$ sudo ln /private/apps/diary/bin/diary /etc/init.d/diary
$ cd /etc/init.d/
$ ls

$ chmod a+x /etc/init.d/diary
$ ls
$ update-rc.d diary defaults
$ service diary start
```

ghost

```
#!/bin/bash
#
# An init.d script for running a Node.js process as a service using Forever as
# the process monitor. For more configuration options associated with Forever,
# see: https://github.com/nodejitsu/forever
#
# This was written for Debian distributions such as Ubuntu, but should still
# work on RedHat, Fedora, or other RPM-based distributions, since none of the
# built-in service functions are used. So information is provided for both.
#
### BEGIN INIT INFO
# Provides:             my-application
# Required-Start:       $syslog $remote_fs
# Required-Stop:        $syslog $remote_fs
# Should-Start:         $local_fs
# Should-Stop:          $local_fs
# Default-Start:        2 3 4 5
# Default-Stop:         0 1 6
# Short-Description:    My Application
# Description:          My Application
### END INIT INFO
#
### BEGIN CHKCONFIG INFO
# chkconfig: 2345 55 25
# description: My Application
### END CHKCONFIG INFO
#
# Based on:
# https://gist.github.com/3748766
# https://github.com/hectorcorrea/hectorcorrea.com/blob/master/etc/forever-initd-hectorcorrea.sh
# https://www.exratione.com/2011/07/running-a-nodejs-server-as-a-service-using-forever/
#
# The example environment variables below assume that Node.js is installed by
# building from source with the standard settings.
#
# It should be easy enough to adapt to the paths to be appropriate to a package
# installation, but note that the packages available in the default repositories
# are far behind the times. Most users will be building from source to get a
# suitably recent Node.js version.
#
# An application name to display in echo text.
# NAME="My Application" 
# The full path to the directory containing the node and forever binaries.
# NODE_BIN_DIR="/usr/local/node/bin" 
# Set the NODE_PATH to the Node.js main node_modules directory.
# NODE_PATH="/usr/local/lib/node_modules" 
# The application startup Javascript file path.
# APPLICATION_PATH="/home/user/my-application/start-my-application.js" 
# Process ID file path.
# PIDFILE="/var/run/my-application.pid" 
# Log file path.
# LOGFILE="/var/log/my-application.log" 
# Forever settings to prevent the application spinning if it fails on launch.
# MIN_UPTIME="5000" 
# SPIN_SLEEP_TIME="2000" 

NAME="" 
NODE_BIN_DIR="" 
NODE_PATH="" 
APPLICATION_PATH="" 
PIDFILE="" 
LOGFILE="" 
MIN_UPTIME="" 
SPIN_SLEEP_TIME="" 

# Add node to the path for situations in which the environment is passed.
PATH=$NODE_BIN_DIR:$PATH
# Export all environment variables that must be visible for the Node.js
# application process forked by Forever. It will not see any of the other
# variables defined in this script.
export NODE_PATH=$NODE_PATH

start() {
    echo "Starting $NAME" 
    # We're calling forever directly without using start-stop-daemon for the
    # sake of simplicity when it comes to environment, and because this way
    # the script will work whether it is executed directly or via the service
    # utility.
    #
    # The minUptime and spinSleepTime settings stop Forever from thrashing if
    # the application fails immediately on launch. This is generally necessary to
    # avoid loading development servers to the point of failure every time
    # someone makes an error in application initialization code, or bringing down
    # production servers the same way if a database or other critical service
    # suddenly becomes inaccessible.
    #
    # The pidfile contains the child process pid, not the forever process pid.
    # We're only using it as a marker for whether or not the process is
    # running.
    #
    # Note that redirecting the output to /dev/null (or anywhere) is necessary
    # to make this script work if provisioning the service via Chef.
    forever \
      --pidFile $PIDFILE \
      -a \
      -l $LOGFILE \
      --minUptime $MIN_UPTIME \
      --spinSleepTime $SPIN_SLEEP_TIME \
      start $APPLICATION_PATH 2>&1 > /dev/null &
    RETVAL=$?
}

stop() {
    if [ -f $PIDFILE ]; then
        echo "Shutting down $NAME" 
        # Tell Forever to stop the process.
        forever stop $APPLICATION_PATH 2>&1 > /dev/null
        # Get rid of the pidfile, since Forever won't do that.
        rm -f $PIDFILE
        RETVAL=$?
    else
        echo "$NAME is not running." 
        RETVAL=0
    fi
}

restart() {
    stop
    start
}

status() {
    # On Ubuntu this isn't even necessary. To find out whether the service is
    # running, use "service my-application status" which bypasses this script
    # entirely provided you used the service utility to start the process.
    #
    # The commented line below is the obvious way of checking whether or not a
    # process is currently running via Forever, but in recent Forever versions
    # when the service is started during Chef provisioning a dead pipe is left
    # behind somewhere and that causes an EPIPE exception to be thrown.
    # forever list | grep -q "$APPLICATION_PATH" 
    #
    # So instead we add an extra layer of indirection with this to bypass that
    # issue.
    echo `forever list` | grep -q "$APPLICATION_PATH" 
    if [ "$?" -eq "0" ]; then
        echo "$NAME is running." 
        RETVAL=0
    else
        echo "$NAME is not running." 
        RETVAL=3
    fi
}

case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    status)
        status
        ;;
    restart)
        restart
        ;;
    *)
        echo "Usage: {start|stop|status|restart}" 
        exit 1
        ;;
esac
exit $RETVAL
```


## 10. Creating Config File

```bash
$ cd /private/apps/diary/diary
$ sudo mkdir config
$ cd config

$ sudo mkdir bin
$ sudo mkdir nginx

$ cd bin
$ sudo cp /private/apps/diary/bin/diary diary-dev

$ cd ..
$ cd nginx
$ sudo cp /private/apps/diary/nginx/diary.conf diary.conf
```

## 11. Git

```bash
$ git init
$ git remote add origin xxxx
$ git add .
$ git commit -m "xxx" 
$ git push origin master
```

## References

- [Installing ghost linux](http://support.ghost.org/installing-ghost-linux/)
