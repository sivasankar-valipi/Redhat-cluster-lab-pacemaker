
In this lab, we will create a Red Hat Enterprise Linux virtual machine named Storage.

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

Note: VMware uses this password to create both the normal user and configure the root account during installation. You can change it later if required.

Click -> Next

**Step 5** : Name the Virtual Machine

Storage

Choose the location where VMware will store the VM files.

C:\VMs\

Click -> Next

**Step 6** : Specify Disk Capacity

Select -> 20 GB

Store virtual disk as a single file

Why?

Storing the virtual disk as a single file generally provides slightly better performance and is easier to manage than splitting it into multiple files.

Click -> Next

**Step 7** : Finish VM Creation

Review the configuration.

Click -> Finish
