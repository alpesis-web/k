---
layout: post_tech
title: "Installing Cloudera Hadoop Cluster with Vagrant VirtualBox and Cloudera Manager"
date: 2014-12-09 10:09:22 +0800
comments: true
categories: [tech]
tags: [cloudera, hadoop, cluster]
toc: true
---

## Requirements

- Memory: 16GB+
- OS: Ubuntu / Centos
- Vagrant
- VirtualBox

## 1. Vagrant

### 1.1. Create VMs

```bash
$ vagrant plugin install vagrant-hostmanager

$ git clone https://github.com/DandyDev/virtual-hadoop-cluster.git
$ cd virtual-hadoop-cluster
$ vagrant up
```

### 1.2. Cluster installation

1 Master 3 Slaves

Master

```bash
apt-get install curl -y
REPOCM=${REPOCM:-cm5}
CM_REPO_HOST=${CM_REPO_HOST:-archive.cloudera.com}
CM_MAJOR_VERSION=$(echo $REPOCM | sed -e 's/cm\\([0-9]\\).*/\\1/')
CM_VERSION=$(echo $REPOCM | sed -e 's/cm\\([0-9][0-9]*\\)/\\1/')
OS_CODENAME=$(lsb_release -sc)
OS_DISTID=$(lsb_release -si | tr '[A-Z]' '[a-z]')
if [ $CM_MAJOR_VERSION -ge 4 ]; then
  cat > /etc/apt/sources.list.d/cloudera-$REPOCM.list <<EOF
deb [arch=amd64] http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm $OS_CODENAME-$REPOCM contrib
deb-src http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm $OS_CODENAME-$REPOCM contrib
EOF
curl -s http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm/archive.key > key
apt-key add key
rm key
fi
apt-get update
export DEBIAN_FRONTEND=noninteractive
apt-get -q -y --force-yes install oracle-j2sdk1.7 cloudera-manager-server-db cloudera-manager-server cloudera-manager-daemons
service cloudera-scm-server-db initdb
service cloudera-scm-server-db start
service cloudera-scm-server start
```

Slave

```bash
cat > /etc/hosts <<EOF
127.0.0.1       localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

EOF
```

## 2. Cluster with Cloudera Manager

open the site: http://vm-cluster-node1:7180

- username/password: admin/admin
- select `Cloudera Express` or `Cloudera Professional`, click `continue`
- fill the form: `hosts: vm-cluster-node[1-4]`, click `search`, click `continue`
- `Cluster Installation > Select Repository`, default settings, click `continue`
- `Cluster Installation > Configure Java Encryption`, click `install Java (without encryption)`, click `continue`
- choose login user: `Another user`, fill in `vagrant`, password `vagrant`, click `continue`
- installing, once done, click `continue`
- downloading CDH, once done, click `continue`
- checkint hosts, if flag some errors, click `Run Again`, once done, click `finished`
- choose all components, click `continue`
- default settings, click `continue`
- `Database Setup`, select `Use Embeded Database`, chick "Test Connection", click `Continue`
- `Review Changes`, config the services
- done

## 3. Testing

- site: http://vm-cluster-node1:7180
- username/password: admin/admin


## Trouble Shooting

when checking the hosts, cloudera manager flags some warning:

```
Cloudera 建议将 /proc/sys/vm/swappiness 设置为 0。当前设置为 60。使用 sysctl 命令在运行时更改该设置并编辑 /etc/sysctl.conf 以在重启后保存该设置。
您可以继续进行安装，但可能会遇到问题，Cloudera Manager 报告您的主机由于交换运行状况不佳。以下主机受到影响： 
vm-cluster-node[1-4]
```

Solution

```bash
$ vagrant ssh master/slave1/slave2/slave3
$ sudo nano /proc/sys/vm/swappiness
$ # update the value as 0, save and exit
```

Applied to all VMs.

## Reference

- [how-to-install-a-virtual-apache-hadoop-cluster-with-vagrant-and-cloudera-manager](http://blog.cloudera.com/blog/2014/06/how-to-install-a-virtual-apache-hadoop-cluster-with-vagrant-and-cloudera-manager/)
