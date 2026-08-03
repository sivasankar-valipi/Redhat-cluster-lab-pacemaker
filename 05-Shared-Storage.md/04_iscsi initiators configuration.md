## What is an Initiator?

The Initiator is the client that requests storage from the iSCSI Target.

#### In our lab:

storage.lab.local  ---> iSCSI Target

node1              ---> Initiator

node2              ---> Initiator

node3              ----> Initiator

## Steps to install and configure iscsi initiator on nodes

**Step 1:** Install iscsi-initiator software

yum install -y iscsi-initiator-utils

It Installs: 

iscsiadm - CLI tool

iscsiadm - daemon

configuration files and services

**Step 2:** Start and enable the service

systemctl start iscsid

systemctl enable iscsid --now

**Step 3:** Verify the service status

systemctl status iscsid

**Step 4**: Discover the target

iscsiadm -m discovery -t sendtargets -p 192.168.43.130

-m discovery: I don't want to login yet, just want to know what targets are available

-t sendtargets: List of available iscsi targets

-p IP Address: Storage server 

**Step 6:** Login into the target

iscsiadm -m node --login

**Step 7:** Verify the session

iscsiadm -m session

**Step 8:** Verify disk

lsblk


#### To logout from session

iscsiadm -m node --logout
