# Configure Network Adapters

After logging in,

Check the IP address, ***ip addr or ip a***

Initially, you'll notice only one network interface, typically ens160, which is connected to the NAT network.

To confirm this, check the routing table:

***ip route***

default via 192.168.237.2 dev ens160

The default gateway points to the NAT network, indicating that ens160 is responsible for internet access.

Power Off the Virtual Machine

## Before adding another network adapter, shut down the VM cleanly:

shutdown -h now

### In VMware Workstation:

Right-click the virtual machine.

Select Settings.

Go to Network Adapter.

Ensure Network Adapter 1 is set to NAT.

Click Add.

Select Network Adapter.

Click Finish.

Choose Host-only for the new adapter.

Why Host-only?

The Host-only network creates a private network shared only between the host system and the virtual machines. This network is isolated from external systems and is ideal for cluster heartbeat and node-to-node communication.

Using a dedicated private network ensures:

Reliable heartbeat communication
Low latency
Isolation from production or internet traffic
Improved cluster stability
Power On the Virtual Machine

Start the VM again.

Verify the available network interfaces:

ip addr

You should now see two interfaces:

ens160
ens224

Typically:

ens160 → NAT (Internet access)

ens224 → Host-only (Cluster communication)

To identify which interface is connected to NAT, check the routing table:

ip route

default via 192.168.237.2 dev ens160

Since the default route uses ens160, it is the NAT interface. The Host-only interface (ens224) will not have a default gateway because it is intended solely for private communication between cluster nodes.

 ## Node1 Configuration:
 
Virtual Disk - 30GB

OS - Red Hat Enterprise Linux 9.8

Host Name - node1.lab.local

User Name - redhat

Software Selection - Minimal Install

Adapter 1: NAT (Internet access for installing packages)

IP Address -192.168.237.128/24 - ens160

Subnet - 255.255.255.0

Gateway - 192.168.237.2

DNS - 192.168.237.2 or 8.8.8.8

Adapter 2: Host-only (private cluster communication)

IP Address -192.168.43.128/24 - ens224

Subnet -255.255.255.0

Gateway - None

DNS - None

## Node2 Configuration:

Virtual Disk - 30GB

OS - Red Hat Enterprise Linux 9.8

Host Name - node2.lab.local

User Name - redhat

Software Selection - Minimal Install

Adapter 1: NAT (Internet access for installing packages)

IP Address -192.168.237.129/24 - ens160

Subnet - 255.255.255.0

Gateway -192.168.237.2

DNS - 192.168.237.2 or 8.8.8.8

Adapter 2: Host-only (private cluster communication)

IP Address -192.168.43.129/24 - ens224

Subnet -255.255.255.0

Gateway - None

DNS - None

## Node3 Configuration:
 
Virtual Disk - 30GB

OS - Red Hat Enterprise Linux 9.8

Host Name - node3.lab.local

User Name - redhat

Software Selection - Minimal Install

Adapter 1: NAT (Internet access for installing packages)

IP Address -192.168.237.130/24 - ens160

Subnet - 255.255.255.0

Gateway - 192.168.237.2

DNS - 192.168.237.2 or 8.8.8.8

Adapter 2: Host-only (private cluster communication)

IP Address -192.168.43.130/24 - ens224

Subnet -255.255.255.0

Gateway - None

DNS - None

                    NAT Network
                 192.168.237.0/24
                         │
        ┌────────────────┼────────────────┐
        │                │                │
      node1            node2            node3
      ens160           ens160           ens160
192.168.237.128   192.168.237.129   192.168.237.130


                  Host-only Network
                  192.168.43.0/24
        ┌────────────────┼────────────────┐
        │                │                │
      node1            node2            node3
      ens224           ens224           ens224
192.168.43.128    192.168.43.129    192.168.43.130
