---
layout: post_tech
title: "Installing Logstack ElasticSearch Kibana on Ubuntu"
date: 2015-12-29 14:52:55 +0800
comments: true
categories: [tech]
tags: [java, logstack, elasticsearch, kibana, nginx, ssl, filebeat]
toc: true
---

## Installation Steps

- Step 1. java
- Step 2. elasticsearch
- Step 3. kibana/nginx
- Step 4. logstack
- Step 5. filebeat

## Process

Applications:

- filebeat: ship logs
- logstack: process and index logs
- elasticsearch: store logs
- kibana: search and visualize logs
- nginx: reverse proxy

Process:

```
App Server - filebeat -|
                       |-> logstack -> elasticsearch -> kibana -> nginx
DB Server  - filebeat -|
```


## Prequisitions

- Ubuntu 14.04
- Java 8
- Logstack
- ElasticSearch
- Kibana
- Filebeat

## Java 8

```bash
$ sudo add-apt-repository -y ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get -y install oracle-java8-installer
```

## ElasticSearch

### Install ElasticSearch

```bash
$ wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list
$ sudo apt-get update
$ sudo apt-get -y install elasticsearch

$ sudo vim /etc/elasticsearch/elasticsearch.yml
# network.host: 192.168.0.1
# network.host: localhost
network.host: 0.0.0.0

$ sudo service elasticsearch restart
$ sudo update-rc.d elasticsearch defaults 95 10
```

Website: http://localhost:9200

<img src="https://s-media-cache-ak0.pinimg.com/736x/b0/81/cc/b081cc477c42be5c5d48f55fed6c6a09.jpg" />

### Install ElasticSearch-Head

```bash
$ sudo /usr/share/elasticsearch/bin/plugin install mobz/elasticsearch-head
```

Website: http://localhost:9200/_plugin/head/

<img src="https://s-media-cache-ak0.pinimg.com/736x/24/94/7c/24947c5722c70134ef4bd199fe7ca114.jpg" />


## Kibana

```bash
$ sudo groupadd -g 1999 kibana
$ sudo useradd -u 1999 -g 1999 kibana

$ cd /tmp
$ wget https://download.elastic.co/kibana/kibana/kibana-4.3.0-linux-x64.tar.gz
$ tar xvf kibana-*.tar.gz
$ cd kibana-4.3.0-linux-x64
$ vim config/kibana.yml      # update some configurations


$ sudo mkdir -p /opt/kibana
$ sudo cp -R kibana-4.3.0-linux-x64/* /opt/kibana/
$ ls /opt/kibana/
$ sudo chown -R kibana: /opt/kibana

$ cd /etc/init.d && sudo curl -o kibana https://gist.githubusercontent.com/thisismitch/8b15ac909aed214ad04a/raw/fc5025c3fc499ad8262aff34ba7fde8c87ead7c0/kibana-4.x-init
$ cd /etc/default && sudo curl -o kibana https://gist.githubusercontent.com/thisismitch/8b15ac909aed214ad04a/raw/fc5025c3fc499ad8262aff34ba7fde8c87ead7c0/kibana-4.x-default 

$ sudo chmod +x /etc/init.d/kibana
$ sudo update-rc.d kibana defaults 96 9
$ sudo service kibana start
```

test: http://localhost:5601

<img src="https://s-media-cache-ak0.pinimg.com/736x/cf/05/2c/cf052c0e3e1b3781238082cb844e33e1.jpg" />

### Nginx

```bash
$ sudo apt-get install nginx apache2-utils
$ sudo htpasswd -c /etc/nginx/htpasswd.users kibanaadmin

$ sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/kibana # update config
$ sudo ln /etc/nginx/sites-available/kibana /etc/nginx/sites-enabled/kibana
$ sudo service nginx restart
```

/etc/nginx/sites-available/kibana

```bash
server {
    listen 80;

    server_name example.com;

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;        
    }
}
```


## Logstack

### Install Logstack

```bash
$ echo 'deb http://packages.elasticsearch.org/logstash/2.1/debian stable main' | sudo tee /etc/apt/sources.list.d/logstash.list
$ sudo apt-get update
$ sudo apt-get install logstash
```

### SSL Certificate

```bash
$ sudo mkdir -p /etc/pki/tls/certs
$ sudo mkdir /etc/pki/tls/private
$ cd /etc/pki/tls; sudo openssl req -subj '/CN=logstash_server_fqdn/' -x509 -days 3650 -batch -nodes -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt
```

### Config Logstack

- /etc/logstash/conf.d/02-filebeat-input.conf
- /etc/logstash/conf.d/10-syslog.conf
- /etc/logstash/conf.d/30-elasticsearch-output.conf

```bash
$ sudo vi /etc/logstash/conf.d/02-filebeat-input.conf
```

02-filebeat-input.conf

```bash
input {
  beats {
    port => 5044
    type => "logs"
    ssl => true
    ssl_certificate => "/etc/pki/tls/certs/logstash-forwarder.crt"
    ssl_key => "/etc/pki/tls/private/logstash-forwarder.key"
  }
}
```

```bash
$ sudo vi /etc/logstash/conf.d/10-syslog.conf
```

10-syslog.conf

```bash
filter {
  if [type] == "syslog" {
    grok {
      match => { "message" => "%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:syslog_hostname} %{DATA:syslog_program}(?:\[%{POSINT:syslog_pid}\])?: %{GREEDYDATA:syslog_message}" }
      add_field => [ "received_at", "%{@timestamp}" ]
      add_field => [ "received_from", "%{host}" ]
    }
    syslog_pri { }
    date {
      match => [ "syslog_timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
  }
}
```

```bash
$ sudo vi /etc/logstash/conf.d/30-elasticsearch-output.conf
```

30-elasticsearch-output.conf

```bash
output {
  elasticsearch { hosts => ["localhost:9200"] }
  stdout { codec => rubydebug }
}
```

test

```bash
$ sudo service logstash configtest
$ sudo service logstash restart
$ sudo update-rc.d logstash defaults 96 9
```

## Filebeat (Client)

### SSL Certificate

```bash
$ scp /etc/pki/tls/certs/logstash-forwarder.crt user@client_server_private_address:/tmp
```

### Install Filebeat

```bash
$ echo "deb https://packages.elastic.co/beats/apt stable main" |  sudo tee -a /etc/apt/sources.list.d/beats.list
$ wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install filebeat

$ sudo mkdir -p /etc/pki/tls/certs
$ sudo cp /tmp/logstash-forwarder.crt /etc/pki/tls/certs/
```

### Config Filebeat


```bash
$ sudo vi /etc/filebeat/filebeat.yml
```

/etc/filebeat/filebeat.yml

```bash
...
      paths:
         - /var/log/auth.log
         - /var/log/syslog
#        - /var/log/*.log
...

...
      document_type: syslog
...

...
output:

  ### Elasticsearch as output
  elasticsearch:
    enabled: false
...

  ### Logstash as output
  logstash:
    # The Logstash hosts
    hosts: ["ELK_server_private_IP:5044"]

...
    tls:
      # List of root certificates for HTTPS server verifications
      certificate_authorities: ["/etc/pki/tls/certs/logstash-forwarder.crt"]

```

test

```bash
$ sudo service filebeat restart
$ sudo update-rc.d filebeat defaults 95 10
```
