### Overview

Apache is one of the most commonly clustered applications in enterprise environments.

In this chapter, we will configure Apache HTTP Server as a Pacemaker-managed resource. Instead of starting and stopping Apache manually using systemctl, Pacemaker will control the service through the Apache Resource Agent.

If the active node fails, Pacemaker will automatically start Apache on another healthy node, ensuring that the web application remains available.

#### Resource Agent

Apache is managed by the following Resource Agent:

ocf:heartbeat:apache

#### Prerequisites

Before creating the Apache resource, ensure:

Apache package is installed on all cluster nodes.

The Apache configuration file is valid.

Port 80 is allowed through the firewall if required.

The web content is available (local disk or shared storage).

The Virtual IP resource is already configured and working.

****Important: Every node that may host the Apache resource must have Apache installed and configured. During failover, Pacemaker may start the service on any eligible node.****

## Create Apache Cluster Resource 

***Syntax: pcs resource create <Resource_Name> ocf:heartbeat:apache configfile=<Apache_Config_File> statusurl=<Apache_Status_URL> op monitor interval=<Time>***

pcs resource create webserver ocf:heartbeat:apache op monitor interval=30s

****Note: Apache=httpd****

#### Verifying the Resource

--> pcs status

--> pcs resource status

--> systemctl status httpd

--> curl http://192.168.43.154

#### Summary:

In this chapter, you created Apache HTTP Server as a Pacemaker-managed resource using the ocf:heartbeat:apache Resource Agent. The Apache resource is now controlled by Pacemaker, which can start, stop, monitor, and recover the service automatically. By combining this resource with the Virtual IP created earlier, you now have the foundation for a highly available web application.
