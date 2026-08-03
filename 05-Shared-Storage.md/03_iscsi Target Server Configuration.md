Before we create the iSCSI target, we will learn about objects inside an iscsi target.

**1.Backstore**

A ***Backstore*** is the storage object that the iSCSI target exports. 

A backstore can be:

Entire disk (/dev/sda)

Partition (/dev/sda1)

LVM Logical Volume

File-backed image

RAM disk

**2.iSCSI Target (IQN)**

iSCSI Target

↓

Connection Point

It doesn't contain storage yet.

**3.Logical Unit Number (LUN)**

The LUN is what the client eventually sees as a disk.

**4.Access Control List (ACL)**

We decide which initiators are allowed.

Example:

Allowed

node1 IQN

node2 IQN

node3 IQN

**5.Portal**

The target needs a network address.

192.168.43.130:3260

IP Address → 192.168.43.130

TCP Port → 3260 (default iSCSI port)

This is where initiators connect.

**6.Target Portal Group (tpg)**

It holds:

LUNs

ACLs

Portals

Authentication settings

***Architecture:***

                 Storage Server

                     /dev/sdb
                        │
                        ▼
                 +--------------+
                 |  Backstore   |
                 +--------------+
                        │
                        ▼
           +---------------------------+
           | iSCSI Target (IQN)        |
           +---------------------------+
                        │
                 +------+------+
                 |   LUN0      |
                 +------+------+
                        │
          +-------------+----------------------+ 
          |                    |               |
       ACL (node1)        ACL (node2)      ACL (node3)
          |                    |               |
          +-------------+----------------------+
                        │
                 Portal 192.168.43.130:3260
                        │
                 Ethernet Network
                        │
               +--------+--------+
               |        |        |
            node1     node2     node3

## Steps to install and configure iscsi target server on storage node
                    
                    Host-only Network (192.168.43.0/24)

                 +----------------------------+
                 |                            |
                 |                            |
         +-------+--------+          +--------+-------+
         |    node1       |          |     node2      |
         |192.168.43.128  |          |192.168.43.129  |
         +-------+--------+          +--------+-------+
                 \                         /
                  \                       /
                   \                     /
                    \                   /
                  +---------------------------+
                  |   storage.lab.local       |
                  |   192.168.43.130          |
                  |                           |
                  |  targetcli                |
                  |  /dev/sdb (5GB)           |
                  +---------------------------+


**Step 1:** Install the iSCSI Target Software

yum install -y targetcli

What is targetcli?

targetcli manages the Linux iSCSI target.

It lets you:

Create storage objects

Create iSCSI targets

Create LUNs

Map LUNs to targets

Control which clients can connect

**Step 2:** Start and Enable the Target Service

systemctl start target

systemctl enable --now target

Verify:

systemctl status target

**Step 3:**  Run --> targetcli

/>ls

o- /
  o- backstores
  
  o- iscsi
  
  o- loopback

**Step 4:** Create a Backstore named shared_disk using /dev/sda

cd /backstores

cd block

create shared_disk /dev/sda

**Step 5:** Create an iSCSI Target (IQN)

cd /

cd iscsi

create iqn.2026-07-lab.local:storage

**Step 6:** Create LUN0

cd iscsi/iqn.2026-07.lab.local:storage/tpg1/luns

create /backstores/block/shared_disk

**Step 7:** Verify Portal

cd /iscsi/iqn.2026-07.lab.local:storage/tpg1/portals

ls

o- portals
   o- 0.0.0.0:3260  (iSCSI uses port-3260)

**Step 8:** Configure ACLs

cd /iscsi/iqn.2026-07.lab.local:storage/tpg1/acls

create initiator IQN 

We can get Initiator IQN from Nodes by below command

cat /etc/iscsi/initiatorname.iscsi

***In our Lab***

create iqn.1994-05.com.redhat:74if263h657

create iqn.1994-05.com.redhat:b1df6edb8ej

create iqn.1994-05.com:redhat:9l4efh67k286

**Step 9:** Save and Exit

cd /

saveconfig

exit
