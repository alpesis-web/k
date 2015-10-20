---
layout: post_tech
title: "OpenStack Architecture"
date: 2015-10-20 21:16:40 +0800
comments: true
categories: [tech]
tags: [openstack, cloud]
toc: true
---

**NOTE: Here is the study note quoted from OpenStack official site.**


OpenStack is an open source cloud computing platform for all types of clouds, which 
aims to be simple to implement, massively scalable, and feature rich.

It provides an Infrastructure-as-a-Service (IaaS) solution through a set of interrelated 
services. Each service offers an application programming interface (API) that facilitates 
this integration.


## 1. Architecture

### 1.1. Conceptual architecture

<img src="https://s-media-cache-ak0.pinimg.com/736x/75/71/1a/75711ac48a6bdac83020ae2677064023.jpg" />

### 1.2. Logical architecture

<img src="https://s-media-cache-ak0.pinimg.com/originals/19/ad/48/19ad483814fe6454cfc2f57e08c808b5.png" />

## 2. Services

| Service                 | Project    | Description                                            |
|:------------------------|:-----------|:-------------------------------------------------------|
| Dashboard               | Horizon    | provides a web-based portal to interact services       |
| Compute                 | Nova       | manages the lifecycle of compute instances             |
| Networking              | Neutron    | enables Network-Connectivity-as-a-Service              |
| Object Storage          | Swift      | stores and retrieves arbitrary unstructured data obj   |
| Block Storage           | Cinder     | provides persistent block storage to running instances |
| Identity service        | Keystone   | provides an authentication and authorization service   |
| Image service           | Glance     | stores and retrieves virtual machine disk images       |
| Telemetry               | Ceilometer | billing, benchmarking, scalability, and statistics     |
| Orchestration           | Heat       | orchestrates multiple composite cloud applications     |
| Database service        | Trove      | provides scalable and reliable Database-as-a-Service   |
| Data processing service | Sahara     | Provides capabilities to provision and scale Hadoop    |

## 3. Example: OpenStack Networking

<img src="https://s-media-cache-ak0.pinimg.com/736x/73/fe/d4/73fed4f20f920203c5100863008ab115.jpg" />

- Networking
- Node Types
- Components


### 3.1. Networking

Basic Node Deployment

<img src="https://s-media-cache-ak0.pinimg.com/736x/b1/87/f5/b187f5d5009b91f613884506200a8993.jpg" />

Performance Node Deployment

<img src="https://s-media-cache-ak0.pinimg.com/736x/74/e9/9d/74e99d30f3ce216c4ff78c424d26ee1c.jpg" />


### 3.2. Node Types

- Controller
- Compute
- Storage
- Network
- Utility


#### 3.2.1. Controller

<img src="https://s-media-cache-ak0.pinimg.com/736x/ff/67/54/ff67549b6d23bc0f4fde39249ae4aaac.jpg" />

##### 3.2.1.1. Definitions

Controller nodes are responsible for running the management software services needed for the OpenStack environment to function. These nodes:

- Provide the front door that people access as well as the API services that all other components in the environment talk to.
- Run a number of services in a highly available fashion, utilizing Pacemaker and HAProxy to provide a virtual IP and load-balancing functions so all controller nodes are being used.
- Supply highly available "infrastructure" services, such as MySQL and Qpid, that underpin all the services.
- Provide what is known as "persistent storage" through services run on the host as well. This persistent storage is backed onto the storage nodes for reliability.

##### 3.2.1.2. Hareware

- Model: Dell R620
- CPU: 2x Intel® Xeon® CPU E5-2620 0 @ 2.00 GHz
- Memory: 32 GB
- Disk: two 300 GB 10000 RPM SAS Disks
- Network: two 10G network ports

#### 3.2.2. Compute

<img src="https://s-media-cache-ak0.pinimg.com/736x/8e/bd/9f/8ebd9f6888777c9d145fdf321f42bacc.jpg" />

##### 3.2.2.1. Definitions

Compute nodes run the virtual machine instances in OpenStack. They:

- Run the bare minimum of services needed to facilitate these instances.
- Use local storage on the node for the virtual machines so that no VM migration or instance recovery at node failure is possible.

##### 3.2.2.2. Hareware

- Model: Dell R620
- CPU: 2x Intel® Xeon® CPU E5-2650 0 @ 2.00 GHz
- Memory: 128 GB
- Disk: two 600 GB 10000 RPM SAS Disks
- Network: four 10G network ports (For future proofing expansion)


#### 3.2.3. Storage

<img src="https://s-media-cache-ak0.pinimg.com/736x/80/0e/9b/800e9b943e1867b81bf34858a3de4437.jpg" />

##### 3.2.3.1. Definitions

Storage nodes store all the data required for the environment, including disk images in the Image service library, and the persistent storage volumes created by the Block Storage service. Storage nodes use GlusterFS technology to keep the data highly available and scalable.

##### 3.2.3.2. Hareware

- Model: Dell R720xd
- CPU: 2x Intel® Xeon® CPU E5-2620 0 @ 2.00 GHz
- Memory: 64 GB
- Disk: two 500 GB 7200 RPM SAS Disks and twenty-four 600 GB 10000 RPM SAS Disks
- Raid Controller: PERC H710P Integrated RAID Controller, 1 GB NV Cache
- Network: two 10G network ports

#### 3.2.4. Network

<img src="https://s-media-cache-ak0.pinimg.com/736x/35/58/4d/35584d87ab5f1ca74a9ea32dc32d82ea.jpg" />

##### 3.2.4.1. Definitions

Network nodes are responsible for doing all the virtual networking needed for people to create public or private networks and uplink their virtual machines into external networks. Network nodes:

- Form the only ingress and egress point for instances running on top of OpenStack.
- Run all of the environment's networking services, with the exception of the networking API service (which runs on the controller node).

##### 3.2.4.2. Hareware

- Model: Dell R620
- CPU: 1x Intel® Xeon® CPU E5-2620 0 @ 2.00 GHz
- Memory: 32 GB
- Disk: two 300 GB 10000 RPM SAS Disks
- Network: five 10G network ports

#### 3.2.5. Utility

##### 3.2.5.1. Definitions

Utility nodes are used by internal administration staff only to provide a number of basic system administration functions needed to get the environment up and running and to maintain the hardware, OS, and software on which it runs.

These nodes run services such as provisioning, configuration management, monitoring, or GlusterFS management software. They are not required to scale, although these machines are usually backed up.

##### 3.2.5.2. Hareware

- Model: Dell R620
- CPU: 2x Intel® Xeon® CPU E5-2620 0 @ 2.00 GHz
- Memory: 32 GB
- Disk: two 500 GB 7200 RPM SAS Disks
- Network: two 10G network ports

### 3.3. Components

| Component                    | Detail                              |
|:-----------------------------|:------------------------------------|
| OpenStack release            | Havana                              |
| Host operating system        | Red Hat Enterprise Linux 6.5        |
| OpenStack package repository | Red Hat Distributed OpenStack (RDO) |
| Hypervisor                   | KVM                                 |
| Database                     | MySQL                               |
| Message queue                | Qpid                                |
| Networking service           | OpenStack Networking                |
| Tenant Network Separation    | VLAN                                |
| Image service back end       | GlusterFS                           |
| Identity driver              | SQL                                 |
| Block Storage back end       | GlusterFS                           |

#### 3.3.1. Third Party Component Configuration

<table>
<th>Component</th>
<th>Tuning</th>
<th>Availability</th>
<th>Scalability</th>

<tr>
<td>MySQL</td>
<td>binlog-format = row</td>
<td>
Master/master replication. However, both nodes are not used at the same time. 
Replication keeps all nodes as close to being up to date as possible (although 
the asynchronous nature of the replication means a fully consistent state is not 
possible). Connections to the database only happen through a Pacemaker virtual IP, 
ensuring that most problems that occur with master-master replication can be avoided.
</td>
<td>
Not heavily considered. Once load on the MySQL server increases enough that 
scalability needs to be considered, multiple masters or a master/slave setup can be used.
</td>
</tr>

<tr>
<td>Qpid</td>
<td>max-connections=1000 worker-threads=20 connection-backlog=10, sasl security enabled with SASL-BASIC authentication</td>
<td>
Qpid is added as a resource to the Pacemaker software that runs on Controller nodes where Qpid is situated. This ensures only one Qpid instance is running at one time, and the node with the Pacemaker virtual IP will always be the node running Qpid.
</td>
<td>
Not heavily considered. However, Qpid can be changed to run on all controller nodes for scalability and availability purposes, and removed from Pacemaker.
</td>
</tr>

<tr>
<td>HAProxy</td>
<td>maxconn 3000</td>
<td>
HAProxy is a software layer-7 load balancer used to front door all clustered OpenStack API components and do SSL termination. HAProxy can be added as a resource to the Pacemaker software that runs on the Controller nodes where HAProxy is situated. This ensures that only one HAProxy instance is running at one time, and the node with the Pacemaker virtual IP will always be the node running HAProxy.
</td>
<td>
Not considered. HAProxy has small enough performance overheads that a single instance should scale enough for this level of workload. If extra scalability is needed, keepalived or other Layer-4 load balancing can be introduced to be placed in front of multiple copies of HAProxy.
</td>
</tr>

<tr>
<td>Memcached</td>
<td>MAXCONN="8192" CACHESIZE="30457"</td>
<td>
Memcached is a fast in-memory key-value cache software that is used by OpenStack components for caching data and increasing performance. Memcached runs on all controller nodes, ensuring that should one go down, another instance of Memcached is available.
</td>
<td>
Not considered. A single instance of Memcached should be able to scale to the desired workloads. If scalability is desired, HAProxy can be placed in front of Memcached (in raw tcp mode) to utilize multiple Memcached instances for scalability. However, this might cause cache consistency issues.
</td>
</tr>

<tr>
<td>Pacemaker</td>
<td>
Configured to use corosync and cman as a cluster communication stack/quorum manager, and as a two-node cluster.
</td>
<td>
Pacemaker is the clustering software used to ensure the availability of services running on the controller and network nodes:

- Because Pacemaker is cluster software, the software itself handles its own availability, leveraging corosync and cman underneath.
- If you use the GlusterFS native client, no virtual IP is needed, since the client knows all about nodes after initial connection and automatically routes around failures on the client side.
- If you use the NFS or SMB adaptor, you will need a virtual IP on which to mount the GlusterFS volumes.
</td>
<td>
If more nodes need to be made cluster aware, Pacemaker can scale to 64 nodes.
</td>
</tr>

<tr>
<td>GlusterFS</td>
<td>
glusterfs performance profile "virt" enabled on all volumes. Volumes are setup in two-node replication.
</td>
<td>
Glusterfs is a clustered file system that is run on the storage nodes to provide persistent scalable data storage in the environment. Because all connections to gluster use the gluster native mount points, the gluster instances themselves provide availability and failover functionality.
</td>
<td>
The scalability of GlusterFS storage can be achieved by adding in more storage volumes.
</td>
</tr>

</table>


#### 3.3.2. OpenStack Component Configuration

<table>
<tr>
<th>Component</th>
<th>Node Type</th>
<th>Tuning</th>
<th>Availability</th>
<th>Scalability</th>
</tr>


<tr>
<td>Dashboard (horizon)</td>
<td>Controller</td>
<td>
Configured to use Memcached as a session store, neutron support is enabled, can_set_mount_point = False
</td>
<td>
The dashboard is run on all controller nodes, ensuring at least one instance will be available in case of node failure. It also sits behind HAProxy, which detects when the software fails and routes requests around the failing instance.
</td>
<td>
The dashboard is run on all controller nodes, so scalability can be achieved with additional controller nodes. HAProxy allows scalability for the dashboard as more nodes are added.
</td>
</tr>

<tr>
<td>Identity (keystone)</td>
<td>Controller</td>
<td>
Configured to use Memcached for caching and PKI for tokens.
</td>
<td>
Identity is run on all controller nodes, ensuring at least one instance will be available in case of node failure. Identity also sits behind HAProxy, which detects when the software fails and routes requests around the failing instance.
</td>
<td>
Identity is run on all controller nodes, so scalability can be achieved with additional controller nodes. HAProxy allows scalability for Identity as more nodes are added.
</td>
</tr>

<tr>
<td>Image service (glance)</td>
<td>Controller</td>
<td>
/var/lib/glance/images is a GlusterFS native mount to a Gluster volume off the storage layer.</td>
<td>
The Image service is run on all controller nodes, ensuring at least one instance will be available in case of node failure. It also sits behind HAProxy, which detects when the software fails and routes requests around the failing instance.
</td>
<td>
The Image service is run on all controller nodes, so scalability can be achieved with additional controller nodes. HAProxy allows scalability for the Image service as more nodes are added.
</td>
</tr>

<tr>
<td>Compute (nova)</td>
<td>Controller, Compute</td>
<td>
Configured to use Qpid, qpid_heartbeat = 10, configured to use Memcached for caching, configured to use libvirt, configured to use neutron.

Configured nova-consoleauth to use Memcached for session management (so that it can have multiple copies and run in a load balancer).
</td>
<td>
The nova API, scheduler, objectstore, cert, consoleauth, conductor, and vncproxy services are run on all controller nodes, ensuring at least one instance will be available in case of node failure. Compute is also behind HAProxy, which detects when the software fails and routes requests around the failing instance.

Nova-compute and nova-conductor services, which run on the compute nodes, are only needed to run services on that node, so availability of those services is coupled tightly to the nodes that are available. As long as a compute node is up, it will have the needed services running on top of it.
</td>
<td>
The nova API, scheduler, objectstore, cert, consoleauth, conductor, and vncproxy services are run on all controller nodes, so scalability can be achieved with additional controller nodes. HAProxy allows scalability for Compute as more nodes are added. The scalability of services running on the compute nodes (compute, conductor) is achieved linearly by adding in more compute nodes.
</td>
</tr>

<tr>
<td>Block Storage (cinder)</td>
<td>Controller</td>
<td>
Configured to use Qpid, qpid_heartbeat = 10, configured to use a Gluster volume from the storage layer as the back end for Block Storage, using the Gluster native client.
</td>
<td>
Block Storage API, scheduler, and volume services are run on all controller nodes, ensuring at least one instance will be available in case of node failure. Block Storage also sits behind HAProxy, which detects if the software fails and routes requests around the failing instance.
</td>
<td>
Block Storage API, scheduler and volume services are run on all controller nodes, so scalability can be achieved with additional controller nodes. HAProxy allows scalability for Block Storage as more nodes are added.
</td>
</tr>

<tr>
<td>OpenStack Networking (neutron)</td>
<td>Controller, Compute, Network</td>
<td>
Configured to use QPID, qpid_heartbeat = 10, kernel namespace support enabled, tenant_network_type = vlan, allow_overlapping_ips = true, tenant_network_type = vlan, bridge_uplinks = br-ex:em2, bridge_mappings = physnet1:br-ex
</td>
<td>
The OpenStack Networking service is run on all controller nodes, ensuring at least one instance will be available in case of node failure. It also sits behind HAProxy, which detects if the software fails and routes requests around the failing instance.

OpenStack Networking's ovs-agent, l3-agent, dhcp-agent, and metadata-agent services run on the network nodes, as lsb resources inside of Pacemaker. This means that in the case of network node failure, services are kept running on another node. Finally, the ovs-agent service is also run on all compute nodes, and in case of compute node failure, the other nodes will continue to function using the copy of the service running on them.
</td>
<td>
The OpenStack Networking server service is run on all controller nodes, so scalability can be achieved with additional controller nodes. HAProxy allows scalability for OpenStack Networking as more nodes are added. Scalability of services running on the network nodes is not currently supported by OpenStack Networking, so they are not be considered. One copy of the services should be sufficient to handle the workload. Scalability of the ovs-agent running on compute nodes is achieved by adding in more compute nodes as necessary.
</td>
</tr>

</table>


## Reference

- [OpenStack Ops Architecture](http://docs.openstack.org/openstack-ops/content/architecture.html)
- [OpenStack Admin Guide Cloud](http://docs.openstack.org/admin-guide-cloud/common/conventions.html)
