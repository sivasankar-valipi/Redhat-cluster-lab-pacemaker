After installing the High Availability packages (pcs, pacemaker, and corosync), the next step is to start the pcsd service on every cluster node.

Although Pacemaker and Corosync are the core components of a Red Hat High Availability cluster, they are not started immediately.
Instead, Red Hat requires that the pcsd service be running first because it is responsible for securely configuring and managing the cluster.

The pcs command-line utility communicates with pcsd, and pcsd performs cluster configuration tasks on behalf of the administrator.
Once all nodes have been authenticated and configured, pcsd generates the required cluster configuration files, after which Pacemaker and Corosync can safely start.

## What is pcsd?

pcsd **(PCS Daemon)** is a background service installed with the pcs package. It runs on every cluster node and provides a secure interface for remote cluster management.

Instead of directly modifying configuration files or starting cluster services manually, administrators use the pcs command. 
The pcs command communicates with the pcsd daemon over the network, and pcsd performs the requested operations locally on each node.

Think of pcsd as the management layer between the administrator and the cluster software.

### Responsibilities of pcsd

1.Authenticates cluster nodes

2.Receives commands from the pcs utility

3.Synchronizes cluster configuration across all nodes

4.Generates and updates the Corosync configuration

5.Starts and stops cluster services when requested

6.Provides a secure HTTPS-based management interface

### Relationship Between pcs and pcsd

The following diagram illustrates how the pcs command interacts with the pcsd daemon.

                     Administrator
                           │
                           │
                     pcs Command
                           │
             HTTPS (TCP Port 2224)
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
     pcsd (Node1)                        pcsd (Node2)
        │                                     │
        ├──────── Configures Corosync ────────┤
        ├──────── Starts Pacemaker ───────────┤
        └──────── Synchronizes Cluster ───────┘

Notice that the pcs command never communicates directly with Pacemaker or Corosync. Instead, all management operations are handled through the pcsd daemon.

If Pacemaker or Corosync were started before the cluster configuration was created, they would have no information about:

Which nodes belong to the cluster

Cluster name

Communication network

Quorum settings

Authentication keys

Resource configuration

#### Start the pcsd Service

sudo systemctl start pcsd

#### Enable pcsd at Boot

sudo systemctl enable --now pcsd

#### Verify the Service Status

systemctl status pcsd

Output

● pcsd.service - PCS GUI and remote configuration service

   Loaded: loaded (/usr/lib/systemd/system/pcsd.service)
   
   Active: active (running)

The Active: active (running) status indicates that the daemon is ready to receive cluster management requests.

**PCS** communicates with pcsd over HTTPS, verify that the daemon is listening on TCP port 2224.

ss -tulnp | grep 2224

In the next chapter, you'll configure the hacluster account, explain its purpose, set the password on all nodes, and authenticate the nodes using pcs host auth before creating the cluster.
