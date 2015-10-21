---
layout: post_tech
title: "Ubuntu OpenStack Cloud Design"
date: 2015-10-21 09:10:58 +0800
comments: true
categories: [tech]
tags: [openstack, ubuntu, vmware, vsphere]
toc: true
---

**NOTE: This is the study note from Ubuntu official white paper.**

The OpenStack components are installed as Virtual Machines in a vSphere Cluster. This approach provides the following benefits:

- High availability via vSphere HA
- Better use of the hardware
- Flexibility to scale up and scale out easily as required
- Flexibility to adjust the specifications of each component ( RAM, Disk, vCPU, etc. ) 
- Faster deployment times

## 1. OpenStack Design

Logical Ubuntu Openstack Cloud Design

<img src="https://s-media-cache-ak0.pinimg.com/736x/1a/8e/c1/1a8ec1a4f112dfb5f06e5680a5743831.jpg" />

Logical Ubuntu Cloud on vSphere Design

<img src="https://s-media-cache-ak0.pinimg.com/736x/ef/a0/c8/efa0c8bb3cde24a3ecc9c5a489d0d5e4.jpg" />

design notes:

- A floating network (not shown) is optional
- Each vSphere cluster is associated with a nova-compute. One cannot map
multiple clusters to the same nova-compute, otherwise the clusters would 
get merged to look like a single hypervisor thereby removing the option 
of having clusters in different OpenStack availability zones
- This setup allows for one nova service and one nova.conf for both clusters 
and each is represented as a separate nova-compute hypervisor instance to 
the OpenStack Nova scheduler
- As of this writing, using one nova.conf for both clusters is not recommended 
since there is no established method to define clusters into individual OpenStack 
availability zones.
- OpenStack component HA is achieved via Juju and Metal-as-a-Service (MAAS)
- OpenStack services shown in the Management Cluster can be distributed
to other clusters depending on resource availability (not shown)

VMWARE ESXI HYPERVISORS

| VM Attribute         | Specification          |
|:---------------------|:-----------------------|
| Number of CPUs       | 2                      |
| Memory               | 4 GB                   |
| Number of vNIC ports | 1 (Management network) |
| Disk 1               | 20 GB                  |
| Disk 2               | 20 GB                  |

## 2. Network

Virtual networks exist to attach the VMs vNICs to the right physical networks.

These are the vSphere networks for the environment:

<img src="https://s-media-cache-ak0.pinimg.com/736x/f4/e9/67/f4e967f6dfc72b397aabb47107900626.jpg" />

In this design, OpenStack Havana is implemented with nova-network. 

OpenStack Neutron plus VMware NSX would be a recommended next step, but was not selected in this design.

### 2.1. DHCP and DNS DHCP

MAAS dynamically manages DHCP and DNS for all the OpenStack nodes using the Management Network.

The MAAS node will also provide the Ubuntu Precise 12.04 LTS base images to the VMs in the Ubuntu Cloud via PXE boot through the same network.

### 2.2. Management network isolation

This design consists of one main network called the Management Network. Depending on your network configuration, you can connect a cloud portal or clients to this network to access the OpenStack APIs from other networks via routing.

For security reasons this network should be isolated and only accessible from trusted services like a portal or a management client machine.

Because this design is entirely on top of VMware vSphere running nova-network, 
OpenStack security groups are not available. As of this writing, OpenStack 
compute security group functionality is only achievable on vSphere when 
used in combination with VMware NSX SDN solution.



## 3. Storage

Each availability zone should have a Tier 2 SAN with sufficient resources for the planned workload available to be distributed via vSphere datastores to each vSphere cluster.

Notes:

- The vSphere datastores used for the instances should not be used for any other purpose
- Disconnect any other datastore from the ESXi hosts not to be used for the
instances: http://docs.openstack.org/havana/config-reference/content/vmware.html

### 3.1. OpenStack instances storage

The OpenStack Instances are stored in a dedicated vSphere datastore.

### 3.2. Block storage with Cinder using the VMWare driver

OpenStack Cinder is handled using the VMware driver released with OpenStack Havana. 

Note: The current Cinder Juju Charm needs manual configuration after deployment to set up the VMware driver.

### 3.3. Object storage with Ceph rados gateway

A minimal configuration of Object Storage is needed to deploy OpenStack instances via Juju. For that purpose Ceph RADOS Gateway will be deployed with a default configuration in 3 VMs.

Ceph RADOS Gateway will frontend the stored images and OpenStack Glance will point to it.

VM Specifications

| VM Attribute         | Specification          |
|:---------------------|:-----------------------|
| Number of CPUs       | 2                      |
| Memory               | 4 GB                   |
| Number of vNIC ports | 1 (Management network) |
| Disk 1               | 20 GB                  |
| Disk 2               | 20 GB                  |

### 3.4. VM image storage

The storage of the VM templates (images) is handled by the OpenStack Glance. Glance provides multi-tenant image storage services for an OpenStack deployment.

In this design, to maximise availability of the images, Object Storage with Ceph RADOS Gateway will be used.

## Conclusion

This OpenStack reference architecture provides a common abstraction and orchestration layer 
via OpenStack open APIs and Dashboard to control compute workloads while limiting changes 
to pre-existing VMware infrastructure.

This approach allows organizations to extend the ROI of their infrastructure investment 
while developing and enhancing employees’ skills around a next generation platform in 
OpenStack. The cost saving extends further as teams understand the OpenStack paradigm 
enough to determine which workloads/ applications should remain legacy and which ones 
be upgraded to cloud centric fault tolerant designs early in the infrastructure 
migration process.



