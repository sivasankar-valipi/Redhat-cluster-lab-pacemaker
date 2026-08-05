
# High Availability Web Application

## Objective

Deploy a production-style highly available Apache Web Server using:

- Red Hat Enterprise Linux
- Pacemaker
- Corosync
- Apache HTTP Server
- Virtual IP
- Shared XFS Filesystem
- iSCSI Shared Storage

The web application should remain available even if one cluster node fails.
01-Architecture.md
# Architecture
                          Client

                             │

                     Virtual IP (VIP)

                             │

        +----------------+----------------+

        |                |                |

     node1            node2           node3

     Apache           Apache          Apache

     Pacemaker        Pacemaker       Pacemaker

     Corosync         Corosync        Corosync

             \          |           /

              \         |          /

             Shared XFS Filesystem

                     iSCSI LUN

                         │

                storage.lab.local
Components
Component	Purpose
Apache	Web Server
Virtual IP	Client access
SharedFS	Shared website data
Pacemaker	Resource Manager
Corosync	Cluster Communication
iSCSI	Shared Storage
02-Prerequisites.md

Include:

## Cluster Status

✔ Pacemaker Installed

✔ Corosync Installed

✔ Cluster Created

✔ Shared Storage Configured

✔ XFS Filesystem Created

✔ Apache Installed
03-Create-Filesystem-Resource.md

Start with explanation.

## Why do we need a Filesystem Resource?

Pacemaker must control when the shared filesystem is mounted.

The operating system should not automatically mount the shared storage because only the active node must access it.

This prevents data corruption and ensures consistent failover.

Then command.

pcs resource create SharedFS \
 ocf:heartbeat:Filesystem \
 device=/dev/sdb1 \
 directory=/shared_data \
 fstype=xfs

Explain every parameter.

Parameter	Meaning
device	Shared iSCSI LUN
directory	Mount point
fstype	XFS
04-Configure-Apache.md

Explain:

Why change

/var/www/html

to

/shared_data

Then

DocumentRoot "/shared_data"

Explain

<Directory "/shared_data">

Require all granted

</Directory>

Very important.

05-Create-Resource-Group.md

Explain

Why Resource Groups?

SharedFS

↓

ClusterIP

↓

Apache

Command

pcs resource group add WebGroup \
 SharedFS \
 ClusterIP \
 WebServer

Explain ordering.

Mount

↓

VIP

↓

Apache
06-Testing.md

Commands

pcs status

curl localhost

curl http://VIP

mount

ip a

Expected outputs.

07-Failover-Test.md

Scenario

Resources

↓

node3

Then

pcs node standby node3

Expected

SharedFS

↓

node1

Browser

Website still available
08-Troubleshooting.md

This chapter will be gold.

Issue 1

Apache serving old page

Symptoms

Welcome from NODE2

Root Cause

DocumentRoot still pointing to local directory.

Resolution

Update

DocumentRoot "/shared_data"
Issue 2

Apache Default Page

Root Cause

Wrong Directory configuration.

Resolution

<Directory "/shared_data">

Require all granted
Issue 3

403 Forbidden

Root Cause

Directory permissions / SELinux.

Resolution

chmod 755 /shared_data

SELinux

semanage fcontext
restorecon
Issue 4

Resource Agent Timeout

Symptoms

Timed Out

Resolution

pcs resource cleanup WebServer
Issue 5

Port Already in Use

Root Cause

Started Apache using systemctl while Pacemaker already owned the service.

Resolution

Never manage clustered services directly.
