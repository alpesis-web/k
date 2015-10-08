---
layout: post_tech
title: "Installing Openstack Devstack"
date: 2015-10-03 12:33:38 +0800
comments: true
categories: [tech]
tags: [openstack, cloud] 
---

### Prequisition

Softwares:
 
-- (VM) [VirtualBox 5.0][VirtualBox]  
-- (OS) [Ubuntu Server 14.04.3][Ubuntu-Server]  
-- (SW) [Openstack]  

[VirtualBox]: https://www.virtualbox.org/wiki/Downloads
[Ubuntu-Server]: http://www.ubuntu.com/download/server
[Openstack]: https://github.com/openstack-dev/devstack

Steps:

-- Installing VirtualBox  
-- Installing Ubuntu 14.04.03  
-- Installing Openstack  
-- Testing  

### 1. Installing VirtualBox

Go to [VirtualBox], download and install on the local machine.

### 2. Installing Ubuntu

Go to [Ubuntu][Ubuntu-Server] page, download the ubuntu server 14.04.03 (NOTE: Preferring 14.04.03) iso image.

Once done, open VirtualBox, create a vm for Ubuntu server. The vm settings are:  
-- memory: 2GB  
-- storage: 100GB  
-- **network: briaged adapter (IMPORTANT), promiscuous mode with ALLOW ALL**  

Then, attach Ubuntu server iso file to the drive, double click the vm, the system will be jumped to the installation guide.

When installing Ubuntu, the installation mode should be:  
-- **multi-server with MAAS (IMPORTANT)**  

Once done, restart and config the system:  

```bash config system
    $ sudo apt-get update && upgrade
    $ sudo apt-get install git
    $ git config --global user.name "username"
    $ git config --global user.email "user@example.com"
```

### 3. Installing Openstack

Go to [Openstack], download the repo and install:

```bash installing openstack
    # create a project folder and download the repo
    $ cd /
    $ sudo mkdir openstack
    $ cd openstack
    $ git clone https://github.com/openstack-dev/devstack.git
    $ cd devstack
    $ git checkout stable/juno

    # config user
    $ sudo ./tools/create-stack-user.sh
    $ sudo chown -R stack:stack /openstack

    # install
    $ ./stack.sh
    
```

Trouble Shooting

**(1)** If something wrong, try `./unstack.sh` and then `./clean.sh`, restart and then `./stack.sh` again.

**(2)** If re-run `./stack.sh` and the python packages flag some issue, try to delete `/opt/stack/xxx`, and re-run `./stack.sh`.


### 4. Testing

Open a browser, enter the dashboard with `http://ip_address`, login with `admin/password` (NOTE: password was set at previous).



