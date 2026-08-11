After starting the ***pcsd service*** on all cluster nodes, the next step is to configure the ***hacluster user***.

The hacluster account is created automatically when the pcs package is installed. It is a dedicated system account used by the pcsd service to authenticate communication between cluster nodes.

Before a cluster can be created, all nodes must trust each other. This trust is established by configuring the same password for the hacluster user on every node and then authenticating the nodes using the pcs host auth command.

Without successful authentication, cluster configuration commands such as pcs cluster setup will fail because the nodes cannot communicate securely.

***What is the hacluster User?***

The hacluster user is a system account created automatically during the installation of the pcs package.

***You can verify that the account exists by running***

id hacluster

### The following diagram illustrates the authentication process.

```text
                    Administrator
                         │
                         │
                         ▼
              ┌─────────────────────┐
              │     Node1 / Node2   │
              │   pcs host auth     │
              │                     │
              │ HTTPS / TCP 2224    │
              │       (pcsd)        │
              └──────────┬──────────┘
                         │
                         │
              Validate Cluster Password
                         │
                         ▼
                 Mutual Trust
                  Established
                         │
                         ▼
              Node1 ◄──────────► Node2
                   pcsd / HTTPS
```

### Command

```bash
pcs host auth node1 node2 -u hacluster
```

Enter the `hacluster` password when prompted.

The authentication establishes mutual trust between the cluster nodes through `pcsd` over **TCP port 2224**.
             
Once authentication is complete, pcs can securely execute cluster management commands on every node.

***Set the Password***

On Node1:

sudo passwd hacluster

Repeat the same command on:

 -> Node2
 -> Node3

***Note: The password must be identical on every cluster node***

### Authenticate the Cluster Nodes

For a three-node cluster:

sudo pcs host auth node1 node2 node3 -u hacluster

Password:

If authentication succeeds:

node1: Authorized

node2: Authorized

node2: Authorized
