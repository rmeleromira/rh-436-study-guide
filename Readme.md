# EX436 Study Guide

## Red Hat High Availability Clustering
* Red Hat Clustering documentation
  * https://access.redhat.com/documentation/en/red-hat-enterprise-linux/?category=clustering&version=7
* High Availability Add-On Overview
  * https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Overview/ch-introduction-HAAO.html
  * The High Availability Add-On consists of the following major components:
    * Cluster infrastructure — Provides fundamental functions for nodes to work together as a cluster: configuration file management, membership management, lock management, and fencing.
    * High availability Service Management — Provides failover of services from one cluster node to another in case a node becomes inoperative.
    * Cluster administration tools — Configuration and management tools for setting up, configuring, and managing a the High Availability Add-On. The tools are for use with the Cluster Infrastructure components, the high availability and Service Management components, and storage.
  * You can supplement the High Availability Add-On with the following components:
    * Red Hat GFS2 (Global File System 2) — Part of the Resilient Storage Add-On, this provides a cluster file system for use with the High Availability Add-On. GFS2 allows multiple nodes to share storage at a block level as if the storage were connected locally to each cluster node. GFS2 cluster file system requires a cluster infrastructure.
    * Cluster Logical Volume Manager (CLVM) — Part of the Resilient Storage Add-On, this provides volume management of cluster storage. CLVM support also requires cluster infrastructure.
    * Load Balancer Add-On — Routing software that provides high availability load balancing and failover in layer 4 (TCP) and layer 7 (HTTP, HTTPS) services. The Load Balancer Add-On runs in a cluster of redundant virtual routers that uses load algorithms to distribute client requests to real servers, collectively acting as a virtual server. It is not necessary to use the Load Balancer Add-On in conjunction with Pacemaker.
* High Availability Add-On Administration
  * https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Administration/index.html
* High Availability Add-On Reference
  * https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html
* Load Balancer Administration
  * https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Load_Balancer_Administration/index.html
* Global File System 2
  * https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Global_File_System_2/index.html

### A cluster is two or more computers (called nodes or members) that work together to perform a task. There are four major types of clusters:
* #### Storage
  * Storage clusters provide a consistent file system image across servers in a cluster, allowing the servers to simultaneously read and write to a single shared file system. A storage cluster simplifies storage administration by limiting the installation and patching of applications to one file system. Also, with a cluster-wide file system, a storage cluster eliminates the need for redundant copies of application data and simplifies backup and disaster recovery. The High Availability Add-On provides storage clustering in conjunction with Red Hat GFS2 (part of the Resilient Storage Add-On).
* #### High availability
  * High availability clusters provide highly available services by eliminating single points of failure and by failing over services from one cluster node to another in case a node becomes inoperative. Typically, services in a high availability cluster read and write data (by means of read-write mounted file systems). Therefore, a high availability cluster must maintain data integrity as one cluster node takes over control of a service from another cluster node. Node failures in a high availability cluster are not visible from clients outside the cluster. (High availability clusters are sometimes referred to as failover clusters.) The High Availability Add-On provides high availability clustering through its High Availability Service Management component, Pacemaker.
* #### Load balancing
  * Load-balancing clusters dispatch network service requests to multiple cluster nodes to balance the request load among the cluster nodes. Load balancing provides cost-effective scalability because you can match the number of nodes according to load requirements. If a node in a load-balancing cluster becomes inoperative, the load-balancing software detects the failure and redirects requests to other cluster nodes. Node failures in a load-balancing cluster are not visible from clients outside the cluster. Load balancing is available with the Load Balancer Add-On.
* #### High performance
  * High-performance clusters use cluster nodes to perform concurrent calculations. A high-performance cluster allows applications to work in parallel, therefore enhancing the performance of the applications. (High performance clusters are also referred to as computational clusters or grid computing.)

## Install and configure a Pacemaker-based high availability cluster

## Create and manage highly available services
## Troubleshoot common cluster issues
## Work with shared storage (iSCSI) and configure multipathing

## Configure GFS2 file systems

## Keepalived VS Heartbeat/Corosync
* Statefull
  * a cluster-oriented product such as heartbeat will ensure that a shared resource will be present at *at most* one place. This is very important for shared filesystems, disks, etc... It is designed to take a service down on one node and up on another one during a switchover. That way, the shared resource may never be concurrently accessed. This is a very hard task to accomplish and it does it well.
* Stateless
  * a network-oriented product such as keepalived will ensure that a shared IP address will be present at *at least* one place. Please note that I'm not talking about a service or resource anymore, it just plays with IP addresses. It will not try to down or up any service, it will just consider a certain number of criteria to decide which node is the most suited to offer the service. But the service must already be up on both nodes. As such, it is very well suited for redundant routers, firewalls and proxies, but not at all for disk arrays nor filesystems.
    * Examples are APIs
    * Can be load balanced
* http://www.formilux.org/archives/haproxy/1003/3259.html

## TCP balancing (OSI Layer 4) VS Application balancing (Layer 7)
