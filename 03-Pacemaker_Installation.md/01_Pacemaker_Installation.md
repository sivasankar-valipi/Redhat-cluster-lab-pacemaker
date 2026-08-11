A Red Hat High Availability cluster consists of three major components.

### Pacemaker – The Cluster Resource Manager responsible for starting, stopping, monitoring, and recovering applications.
### Corosync – Provides cluster membership, quorum, and reliable communication between nodes.
### pcs – A command-line management tool used to configure and administer the cluster.

### Prerequisites

Before installing the packages, ensure that:

RHEL is installed on all nodes.
The High Availability repository is enabled.
Hostnames resolve correctly.
Nodes can communicate over the private cluster network.
Time synchronization is working.
SSH connectivity configured.

### Verify that the required repositories are enabled.

#### yum repolist

rhel-9-for-x86_64-baseos-rpms

rhel-9-for-x86_64-appstream-rpms

rhel-9-for-x86_64-highavailability-rpms

If the High Availability repository is missing, enable it.

#### subscription-manager repos --enable=rhel-9-for-x86_64-highavailability-rpms

**Check the current subscription status**

Run on Node1, Node2, and Node3:

subscription-manager status

This system is not yet registered.

#### Note: You need a Red Hat account with an active RHEL subscription. For a lab, this could be a Developer Subscription or another eligible subscription.

subscription-manager register

Username: **<your-redhat-username>**

Password: **<your-redhat-password>**

## Install High Availability Packages

Install the required packages on every cluster node.

#### yum install -y pcs pacemaker corosync fence-agents-all

Verify the Installation

#### rpm -q pcs pacemaker corosync fence-agents-all

## pcs

The pcs package provides the command-line interface used to configure and manage the cluster.

It simplifies tasks such as:

1.Creating a cluster

2.Authenticating nodes

3.Managing resources

4.Viewing cluster status

5.Configuring fencing

6.Setting constraints

## Pacemaker

Pacemaker is the Cluster Resource Manager (CRM).

It continuously monitors cluster resources and makes intelligent decisions about:

1.Starting services

2.Stopping services

3.Restarting failed resources

4.Moving resources between nodes

5.Detecting failures

6.Performing automatic failover

Pacemaker acts as the brain of the cluster.

## Corosync

Corosync is responsible for cluster communication.

It provides:

Heartbeat messaging

Cluster membership

Quorum calculation

Reliable messaging

Corosync acts as the communication layer connecting all cluster nodes.

At this stage, these services may not yet be running. They will be configured and started in the next steps.

#### Summary:

The High Availability packages have been installed.

pcs, Pacemaker, and Corosync are available on each cluster node.

The environment is now ready to configure the pcsd service, authenticate cluster nodes, and create the cluster.
