
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

## Cluster Status

✔ Pacemaker Installed

✔ Corosync Installed

✔ Cluster Created

✔ Shared Storage Configured

✔ XFS Filesystem Created

✔ Apache Installed

#### Configure Apache

Perform the following configuration on all three cluster nodes:

Edit the Apache configuration file:

vi /etc/httpd/conf/httpd.conf

Change the DocumentRoot from:

DocumentRoot "/var/www/html"

to:

DocumentRoot "/shared_data"

Update the corresponding <Directory> section to allow Apache to access the shared directory:

<Directory "/shared_data">

Create the Web Page

Because /shared_data is the shared filesystem, you only need to create the website content once.

Log in to any one active cluster node, go to the shared filesystem, and create index.html:

cd /shared_data

echo "Welcome to HA Cluster" > index.html

The file is stored on the shared iSCSI storage, so when Pacemaker moves the filesystem to another node, Apache on that node can access the same index.html.

Important: Do not create separate copies of index.html on each node. The purpose of the shared filesystem is for all cluster nodes to access the same application data.

### Create Cluster Resources

pcs resource create SharedFS ocf:heartbeat:Filesystem device="/dev/sda"  directory="/shared_data"  fstype="xfs" op monitor interval=20s

pcs resource create ClusterIP ocf:heartbeat:IPadrr2 ip=192.168.43.157 subnet=24 op monitor interval=30s

pcs resource create Webserver ocf:heartbeat:apache/httpd op monitor interval=30s

### Create Resource Group

pcs resource group add WebGroup SharedFS ClusterIP WebServer

### verify

pcs status

pcs resource satatus
