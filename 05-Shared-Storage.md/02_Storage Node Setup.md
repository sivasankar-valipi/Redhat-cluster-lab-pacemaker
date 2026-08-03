### The step-by-step procedure for installing Red Hat Enterprise Linux 9.0 on virtual machine and configuring iscsi storage server that will serve as storage node in a Pacemaker/Corosync high-availability cluster.

**Step 1**

Power on the virtual machine.

The system boots from the ISO.

You'll see the Red Hat boot menu.

**Step 2**

Install Red Hat Enterprise Linux 9.0

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

You'll see, VMware Virtual NVMe Disk 20 GB

Select the disk (Double click on it).

Choose --> Automatic Storage Configuration

Why Automatic?

The installer automatically creates the required partitions:

/boot, /, Swap (if needed)

For a lab setup, automatic partitioning is simple and sufficient.

**Step 5**

Open --> Software Selection

Choose --> Minimal Install

Why Minimal Install?

A Red Hat cluster node does not require a graphical desktop environment. Installing only the minimal package set provides several advantages:

Reduced disk usage Lower memory consumption Smaller attack surface Faster boot times Fewer packages to update and maintain

This is the recommended installation type for production cluster nodes.

**Step 6**

Click --> Network & Hostname

Enable the network interface by toggling it ON.

Set the hostname.

storage.lab.local

Click --> Apply

**Step 7**

User Settings

Configure the root password.

Create a standard user if required.

**Step 8**

Click --> Begin Installation

Wait for the installation to complete.

Once finished,

Click --> Reboot System

## Add another Network Adaptor

***In VMware Workstation: ***

Right-click on Storage virtual machine.

Select Settings.

Go to Network Adapter.

Ensure Network Adapter 1 is set to NAT.

Click Add.

Select Network Adapter.

Click Finish.

Choose Host-only for the new adapter.

Click Ok.

## Storage Node Configuration:

Virtual Disk - 20GB (NVMe)

Additional Disk -5GB (SCSI) - /dev/sda

OS - Red Hat Enterprise Linux 9.0

Host Name - storage.lab.local

Username - redhat

Software Selection - Minimal Install

Adapter 1: NAT (Internet access for installing packages)

IP Address -192.168.237.131/24 - ens160

Subnet - 255.255.255.0

Gateway - 192.168.237.2

DNS - 192.168.237.2 or 8.8.8.8

Adapter 2: Host-only (private cluster communication)

IP Address -192.168.43.131/24 - ens224

Subnet -255.255.255.0

Gateway - None

DNS - None

                            NAT Network
                          192.168.237.0/24
        ┌────────────┬────────────┬────────────
        │            │            │            │        
      node1        node2        node3       storage
      ens160       ens160       ens160       ens160
  192.168.237.128 192.168.237.129 192.168.237.130 192.168.237.131


                          Host-only Network
                          192.168.43.0/24
        ┌────────────┬────────────┬────────────┬
        │            │            │            │           
      node1        node2        node3       storage
      ens224       ens224       ens224       ens224
  192.168.43.128 192.168.43.133 192.168.43.132 192.168.43.130

 ### Update /etc/hosts file:

 On all nodes update hosts file with storage ip address

 192.168.43.130 storage.lab.local storage

 ***Verify***

 From Node1: ping storage

 From Node3 or storage: ping Node2

 #### Verify the disks

 lsblk

 NAME   SIZE

NVMe     20G

sda      5G
