## Resource

A ***resource*** is any application, service, IP address, storage, or component that Pacemaker can start, stop, monitor, or move between cluster nodes to ensure high availability.

In a cluster, Pacemaker does not manage the operating system itself. Instead, it manages the resources running on the operating system.

***A resource can represent***

An application, Network service, Virtual IP address, Mounted filesystem, Database, VM, Container

Whenever a node fails, Pacemaker decides where the resource should run and automatically starts it on another healthy node.

### Cluster Resources:

Apache HTTP Server (httpd)

Nginx

MySQL / MariaDB

PostgreSQL

Oracle Database

Virtual IP (VIP)

NFS Server

Samba

Filesystem Mounts

Docker Containers

Virtual Machines (KVM)

Important: Pacemaker does not need to understand how each application works internally. It only needs a Resource Agent (RA) capable of controlling that application.

## Resource Agent (RA)

A ***Resource Agent (RA)*** is a script or executable that knows ***how to manage a specific resource***.

It provides Pacemaker with a standard set of operations such as:

start – Start the resource.

stop – Stop the resource.

monitor – Check whether the resource is healthy.

validate-all – Verify that the resource configuration is valid.

meta-data – Display information about the Resource Agent and its supported parameters.

Pacemaker itself does not know how to start Apache, assign a Virtual IP, or mount a filesystem. Instead, it delegates these tasks to the appropriate Resource Agent.

          Pacemaker
               │
      "Move Virtual IP"
               │
               ▼
       IPaddr2 Resource Agent
               │
               ▼
 Adds VIP to one node and removes it from another

This separation allows Pacemaker to manage many different applications in a consistent way without containing application-specific logic.

### Where Are Resource Agents Stored?

On Red Hat Enterprise Linux, OCF Resource Agents are typically stored under:

ls /usr/lib/ocf/resource.d/

Output:

heartbeat

pacemaker

The heartbeat directory contains many commonly used Resource Agents.

To list them:

ls /usr/lib/ocf/resource.d/heartbeat

Output:

IPaddr2
Filesystem
apache
mysql
nginx
nfsserver
exportfs

Each file in this directory is a Resource Agent responsible for managing a specific resource.

### Resource Agent Standards

Pacemaker uses standardized Resource Agents based on the ***Open Cluster Framework (OCF)*** specification.

A Resource Agent name follows this format:

***ocf:heartbeat:IPaddr2***

Let's break it down:

ocf	Open Cluster Framework standard

heartbeat	Resource Agent provider

IPaddr2	The specific Resource Agent that manages a Virtual IP

ocf:heartbeat:IPaddr2

Use the IPaddr2 Resource Agent provided by the heartbeat provider, following the Open Cluster Framework (OCF) standard.

ocf:heartbeat:apache

manages an Apache web server

ocf:heartbeat:Filesystem

manages a mounted filesystem.

## What is a Virtual IP?

A Virtual IP (VIP) is a floating IP address that is not permanently assigned to any one server or network interface. Instead, it is dynamically managed by Pacemaker as a cluster resource.

Unlike a physical IP address configured in the operating system, a Virtual IP can move between cluster nodes during a failover.

For example:

Node1
------
Physical IP : 192.168.43.128

Node2
------
Physical IP : 192.168.43.129

Virtual IP
-----------
192.168.43.150

Initially, the Virtual IP may be assigned to Node1.

          Clients
              │
              ▼
    Virtual IP (192.168.43.150)
              │
              ▼
            Node1

If Node1 fails, Pacemaker instructs the IPaddr2 Resource Agent to remove the VIP from Node1 and assign it to Node2.

          Clients
              │
              ▼
    Virtual IP (192.168.43.150)
              │
              ▼
            Node2

From the client's perspective, nothing changes—they continue connecting to the same IP address while Pacemaker handles the failover in the background.

### Key Points:
A Resource is anything Pacemaker can manage (start, stop, monitor, or relocate).
A Resource Agent (RA) contains the logic required to control a specific resource.
Pacemaker relies on OCF-compliant Resource Agents instead of implementing application-specific behavior itself.
ocf:heartbeat:IPaddr2 is the Resource Agent responsible for managing Virtual IP addresses.
A Virtual IP provides a stable endpoint for clients while allowing services to move seamlessly between cluster nodes during failover.
