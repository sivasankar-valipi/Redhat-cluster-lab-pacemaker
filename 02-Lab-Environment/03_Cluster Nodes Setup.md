# Install Red Hat Enterprise Linux 9.8

**Steps to follow**

**Step 1**

Power on the virtual machine.

The system boots from the ISO.

You'll see the Red Hat boot menu.

**Step 2**

Install Red Hat Enterprise Linux 9.8

or

Test this media and install Red Hat Enterprise Linux

Which option should we choose?

Choose

Install Red Hat Enterprise Linux

Why?

The "Test this media" option verifies that the ISO image is not corrupted.

Since we downloaded the ISO directly from the Red Hat website, we can safely skip the media test to reduce installation time.

**Step 3**

Language Selection

Choose --> English

**Step 4**

Installation Destination

You'll see, VMware Virtual NVMe Disk 30 GB

Select the disk (Double click on it).

Choose --> Automatic Storage Configuration

Why Automatic?

The installer automatically creates the required partitions:

/boot,
/,
Swap (if needed)

For a lab setup, automatic partitioning is simple and sufficient.

**Step 5**

Open --> Software Selection

Choose --> Minimal Install

Why Minimal Install?

A Red Hat cluster node does not require a graphical desktop environment. Installing only the minimal package set provides several advantages:

Reduced disk usage
Lower memory consumption
Smaller attack surface
Faster boot times
Fewer packages to update and maintain

This is the recommended installation type for production cluster nodes.

**Step 6**

Click --> Network & Hostname

Enable the network interface by toggling it ON.

Set the hostname.

For Node1:

node1.lab.local

For Node2:

node2.lab.local

For Node3:

node3.lab.local

Click --> Apply

**Step 7**

User Settings

Configure the root password.

Create a standard user if required.

**Step 8**

Click  --> Begin Installation

Wait for the installation to complete.

Once finished,

Click --> Reboot System

# Configure Network Adapters

After logging in,

Check the IP address, ip addr or ip a

Initially, you'll notice only one network interface, typically ens160, which is connected to the NAT network.

To confirm this, check the routing table:

ip route

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
