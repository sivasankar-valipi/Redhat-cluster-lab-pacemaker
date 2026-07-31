### Resource

A **resource** is anything pacemaker can start, stop, monitor or move between nodes.

Example:

Apache (httpd), Nginx, MySQL, Oracle Database, VIP, NFS, SAP, SAMBA, File system mounts, Docker Containers, Virtual Machines.

Pacemaker does not care what the application is. if there is a **Resource Agent (RA)** to control it, pacemaker can manage it.

### Resource Agent (RA)

Resource Agent is a script or executable that implements standard operations like start, stop, monitor/validate for specific applications/services. Pacemaker uses Resource Agent to manage resources consistently across the cluster.

Pacemaker itself does not know how to start apache instead it uses Resource Agent.

Resource Agents are stored in ***ls /usr/lib/ocf/resource.d/***

heartbeat

pacemaker

For heartbeat agents list

ls /usr/lib/ocf/resource.d/heartbeat

IPaddr2

File system

apache

MySQL

nginx

### Resource Standars

ocf:heartbeat:IPaddr2

ocf: Open Cluster Framework Standard

heartbeat: Provider Name

IPaddr2: Actual Resource Agent

***Use the IPaddr2 Resource Agent from heartbeat provider following the OCF standard***

What is a Virtual IP?

A Virtual IP (VIP) is an floating IP address that is not permanently bound to a specific network interface or server. Instead, it is managed dynamically by the cluster software.

Unlike a normal IP address configured in the operating system, a Virtual IP is created, assigned, monitored, moved, and removed by Pacemaker through a Resource Agent.
