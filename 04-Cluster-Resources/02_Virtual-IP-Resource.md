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

