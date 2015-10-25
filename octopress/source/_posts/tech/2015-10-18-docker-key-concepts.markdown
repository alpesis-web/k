---
layout: post_tech
title: "Docker Key Concepts"
date: 2015-10-18 14:43:07 +0800
comments: true
categories: [tech]
tags: [docker, devops, vm]
toc: true
---

## 1. Docker Host and Docker Client

### 1.1. Linux

<img src="https://docs.docker.com/installation/images/linux_docker_host.svg" />

(image source: [Docker Official Site](https://docs.docker.com/installation/mac/))

In a Docker installation on Linux, your physical machine is both the localhost and the Docker host. 

- networking, (localhost) your computer, (docker host) the computer which the containers run
- (localhost) runs docker client, docker daemon, any containers
- (port) docker containers using standard `localhost` addressing such as `localhost:8000` or `0.0.0.0:8376`


### 1.2. OS X

<img src="https://docs.docker.com/installation/images/mac_docker_host.svg" />

(image source: [Docker Official Site](https://docs.docker.com/installation/mac/))

In an OS X installation, the docker daemon is running inside a Linux VM called i`default`. 
The `default` is a lightweight Linux VM made specifically to run the Docker daemon on Mac OS X. 
The VM runs completely from RAM, is a small ~24MB download, and boots in approximately 5s.

In OS X, the Docker host address is the address of the Linux VM. 

- OS X <=> VM <=> containers
- When you start the VM with docker-machine it is assigned an IP address. 
- When you start a container, the ports on a container map to ports on the VM. 


## Docker vs Vagrant/VirtualBox + Git

| Docker     | Vagrant         | VirtualBox  | Git   |
|:-----------|:----------------|:------------|:------|
| Dockerfile | Vagrantfile     |             |       |
| image      | box             |             |       |
| container  |                 |             |       |
