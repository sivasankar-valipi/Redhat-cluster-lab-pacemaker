### Creating the Red Hat High Availability Cluster

Creating a **cluster** is much more than grouping servers together. During this process, the nodes exchange authentication information, generate the Corosync configuration, establish cluster membership, and prepare Pacemaker to manage cluster resources.

Once the cluster is created, all nodes work together as a single logical system capable of monitoring resources, detecting failures, and automatically performing failover.

***What Happens When We Create a Cluster***

several operations occur automatically behind the scenes.

1. Creates the Cluster Configuration

   The pcs utility instructs the pcsd daemon on every node to generate the Corosync configuration file.

   /etc/corosync/corosync.conf

   This file contains:

      Cluster name

      Cluster nodes

      Communication addresses

      Quorum configuration

      Corosync transport settings

2. Distributes the Configuration

   After generating the configuration, pcsd copies the same configuration file to every node.

   This guarantees that every cluster member has an identical configuration.

3. Generates Authentication Keys

   Corosync requires secure communication between nodes.

   During cluster setup, an authentication key is generated.

4. Prepares Pacemaker

   Pacemaker is not yet managing resources.

   Instead, it prepares its internal database so that resources can later be added.

5. Creates Cluster Membership

   Corosync now knows:

     Which systems belong to the cluster.

     Which IP addresses should exchange heartbeat packets.

     Which nodes are expected to participate in quorum.

### Create the Cluster

sudo pcs cluster setup mycluster node1 node2 node3

Verify the Configuration File

ls -l /etc/corosync/

### Start the cluster on every node.

sudo pcs cluster start --all

### Enable the Cluster at Boot

sudo pcs cluster enable --all

### Verify Cluster Status

pcs status

All nodes are online.

The cluster has been successfully created.

No application resources have been configured yet.

Understanding the Current DC

One node is displayed as the Current DC.

Example:

**Current DC: node1**

The ***DC (Designated Controller)*** is the Pacemaker node responsible for coordinating cluster decisions.

The DC:

Collects cluster status information.

Decides where resources should run.

Detects failures.

Initiates failover when necessary.

Important: The DC is not a permanent master node. If it fails or leaves the cluster, Pacemaker automatically elects another node as the new DC.

### Verify Corosync Membership

corosync-quorumtool

This confirms that both nodes are active members of the cluster.

### Verify Cluster status

pcs cluster status

### Cluster Architecture
