## What is a Virtual IP?

A Virtual IP (VIP) is a floating IP address that is not permanently assigned to any one server or network interface. Instead, it is dynamically managed by Pacemaker as a cluster resource.

Unlike a physical IP address configured in the operating system, a Virtual IP can move between cluster nodes during a failover.

For example:

Node1:

Physical IP : 192.168.43.128

Node2:

Physical IP : 192.168.43.133

Virtual IP:

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

## Create the Virtual IP Resource

***Syntax: pcs resource create <Resource_Name> ocf:heartbeat:IPaddr2 ip=<Virtual_IP> cidr_netmask=<Netmask> op monitor interval=<Time>***

--> pcs resource create clusterIP ip=192.168.43.154 ocf:heartbeat:IPaddr2 op monitor interval=30s

### Verify the Resource

--> pcs status

Cluster name: mycluster

Node List:

 * Online: [ node1 node2 node3 ]

Full List of Resources:

 * ClusterIP (ocf:heartbeat:IPaddr2): Started node1

--> pcs resource status

--> ip a

## Testing Virtual IP Failover

Put a Node1 into Standby

Instead of shutting down the server, place the active node into standby.

***pcs node standby node1***

When the standby command is executed:

Pacemaker marks Node1 as Standby.

Resources are no longer allowed to run on Node1.

Pacemaker selects another eligible node.

The IPaddr2 Resource Agent removes the VIP from Node1.

The IPaddr2 Resource Agent assigns the VIP to Node2.

A Gratuitous ARP is sent to update the network.

### Failover Workflow:

Administrator

     │
     ▼
     
pcs node standby node1

     │
     ▼
     
Node1 marked Standby

     │
     ▼
     
Pacemaker relocates ClusterIP

     │
     ▼
     
VIP 192.168.43.157 removed from Node1

     │
     ▼
     
VIP 192.168.43.157 assigned to Node2

     │
     ▼
     
Gratuitous ARP Broadcast

     │
     ▼
     
Clients continue using 192.168.43.157


### Verify the Resource Location

--> pcs status

 Output:

Cluster name: mycluster

Node List:

 * Node node1: standby
   
 * Online: [ node2 node3 ]

Full List of Resources:

 * ClusterIP (ocf:heartbeat:IPaddr2): Started node2

The Virtual IP has successfully moved to Node2.

Verify the VIP on Node2

--> ip addr show

The VIP is now assigned to Node2.

***Bring Node1 back online***

pcs node unstandby node1

Verify: pcs status

Node1 is available to host resources again. Whether the VIP moves back automatically depends on your resource configuration, stickiness, and location preferences.

***Important Note: Does the VIP Automatically Move Back to node1?***

Pacemaker avoids unnecessary movement of resources because relocating them can briefly interrupt client connections.

If Node2 is healthy and already hosting the VIP, Pacemaker typically leaves it there unless a policy, constraint, or administrator action requires it to move.

This behavior improves overall service stability.

#### Summary

In this chapter, you verified that the Virtual IP is a truly highly available resource. By placing the active node into standby or stopping the cluster on that node, Pacemaker automatically relocated the VIP to another healthy node. The IPaddr2 Resource Agent handled the network configuration, while Corosync detected the membership change and Pacemaker made the placement decision. From the client's perspective, the service remained reachable through the same IP address, demonstrating transparent failover.
