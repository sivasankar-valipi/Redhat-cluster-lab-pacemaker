# Creating Node1, Node2 and Node3 Virtual Machines

In this lab, we will create a **Red Hat Enterprise Linux ** virtual machine named **Node1**, **Node2** and **Node3**.

---

                         VMware Workstation Pro
                                   │
      -----------------------------------------------------------------
      │                               │                           │
+-------------------------+ +-------------------------+ +-------------------------+
|         Node1           | |         Node2           | |         Node3           |
|-------------------------| |-------------------------| |-------------------------|
| RHEL 9.8                | | RHEL 9.8                | | RHEL 9.8                |
| 2 vCPU                  | | 2 vCPU                  | | 2 vCPU                  |
| 2 GB RAM                | | 2 GB RAM                | | 2 GB RAM                |
| 30 GB Disk              | | 30 GB Disk              | | 30 GB Disk              |
| NAT Adapter             | | NAT Adapter             | | NAT Adapter             |
| Host-Only Adapter       | | Host-Only Adapter       | | Host-Only Adapter       |
+-------------------------+ +-------------------------+ +-------------------------+
---
**Step 1** : Open VMware Workstation Pro

Click -> Create a New Virtual Machine

**Step 2** : Select Configuration Type

Choose -> Typical (Recommended)

Why?

VMware provides two installation methods.

Typical
Custom

For our Red Hat Cluster lab, Typical is sufficient because VMware automatically configures the recommended virtual hardware.

Click -> Next

**Step 3** : Select Installation Media

Choose -> Installer Disc Image File (ISO)

Browse to the downloaded RHEL ISO -> C:\Users\Admin\Downloads\rhel-baseos-9.8-x86_64.iso

Click -> Next

**Step 4** : Easy Install Information

Full Name

Username

Password

Confirm Password

Note: VMware uses this password to create both the normal user and configure the root account during installation (as noted in your lab). You can change it later if required.

Click -> Next

**Step 5** : Name the Virtual Machine

node1

Choose the location where VMware will store the VM files.

C:\VMs\

Click -> Next

**Step 6** : Specify Disk Capacity

30 GB

Select

Store virtual disk as a single file
Why?

Storing the virtual disk as a single file generally provides slightly better performance and is easier to manage than splitting it into multiple files. For a lab environment, this is the recommended option.

Click -> Next

**Step 7** : Finish VM Creation

Review the configuration.

Click -> Finish

# Cloning a Virtual Machine

After successfully installing Red Hat Enterprise Linux 9.8 on Node1, the next step is to create Node2 and Node3 by cloning Node1.

Virtual machine cloning creates an exact copy of an existing virtual machine, including its operating system, installed packages, virtual hardware configuration, and disk contents.
Instead of performing the operating system installation multiple times, cloning allows additional cluster nodes to be created quickly and consistently.

## Why Use Cloning?
1.Saves time by eliminating the need to install RHEL on each node individually.
2.Ensures all cluster nodes have identical operating system versions and package sets.
3.Maintains consistent virtual hardware configurations (CPU, memory, disk, and network adapters).
4.Reduces the risk of configuration differences between cluster nodes.
5.Simplifies the deployment of multi-node cluster environments.

##In VMware Workstation Pro:

Right-click Node1.

Select Manage → Clone.

Click -> Next.

**Step 1**: Select Clone Source

Choose:

The current state in the virtual machine

Click -> Next.

**Step 2**: Select Clone Type

Choose:

Full Clone
Why Full Clone?

A Full Clone creates a completely independent virtual machine with its own virtual disk.

Advantages:

Independent virtual disks
Better performance
No dependency on Node1
Recommended for cluster environments

Click -> Next.

**Step 3**: Name the Virtual Machine

Enter: Node2

Choose the storage location.

C:\VMs\Node2

Click Finish.

Wait until VMware completes the cloning process.

##Follow same process for **Node3** cloning.
