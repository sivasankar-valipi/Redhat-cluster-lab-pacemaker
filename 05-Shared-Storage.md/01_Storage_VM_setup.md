
In this lab, we will create a Red Hat Enterprise Linux virtual machine named **Storage**.

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

**Step 3**: Select Installation Media

Choose -> Installer Disc Image File (ISO)

Browse to the downloaded RHEL ISO -> C:\Users\Admin\Downloads\rhel-baseos-9.8-x86_64.iso

Click -> Next

**Step 4**: Easy Install Information

Full Name

Username

Password

Confirm Password

Note: VMware uses this password to create both the normal user and configure the root account during installation. You can change it later if required.

Click -> Next

**Step 5**: Name the Virtual Machine

Storage

Choose the location where VMware will store the VM files.

C:\VMs\

Click -> Next

**Step 6**: Specify Disk Capacity

Select -> 20 GB

Store virtual disk as a single file

Why?

Storing the virtual disk as a single file generally provides slightly better performance and is easier to manage than splitting it into multiple files.

Click -> Next

**Step 7**: Finish VM Creation

Review the configuration.

Click -> Finish

### Add an Additional Virtual Disk

Before configuring shared storage, add an additional 5 GB virtual disk to the storage server. This disk will be used as the shared storage (iSCSI LUN) that will later be presented to all cluster nodes.

| Disk       | Size      | Purpose                    |
| ---------- | --------- | -------------------------- |
| **Disk 1** | **20 GB** | Operating System (RHEL)    |
| **Disk 2** | **5 GB**  | Shared Storage (iSCSI LUN) |

***Steps to add a New Hard Disk***

Click -> Edit Virtual Machine Settings -> Add -> Select -> Hard Disk -> Next -> Select Disk Type **(SCSI)** -> Next -> Create a new virtual disk -> Next -> Specify Disk Capacity -> Next -> Enter: 5GB -> Select **Store virtual disk as a single file** -> Next-> Choose Disk File Location -> Click -> Finish -> Click -> Ok

The Storage Server now has two virtual disks.
