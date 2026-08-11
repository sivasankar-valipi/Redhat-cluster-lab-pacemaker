---
## Red Hat High Availability (HA) Cluster Architecture

A Red Hat High Availability (HA) Cluster is designed to ensure continuous application availability by eliminating single points of failure.
It consists of multiple Red Hat Enterprise Linux (RHEL) servers, called **nodes**, that work together to provide uninterrupted services.

Pacemaker acts as the cluster manager, while Corosync provides communication between the nodes. If one node fails, Pacemaker automatically starts the resources on another healthy node.

---

## Architecture Diagram

```text
                         Client
                           │
                           │
                  Virtual IP (VIP)
                           │
                  +----------------+
                  |   Pacemaker    |
                  | Cluster Manager|
                  +----------------+
                           │
             Heartbeat & Cluster Communication
                           │
    -----------------------------------------------------------------
    │                         │                         │
    │                         │                         │
+------------------------+ +------------------------+ +------------------------+
|        Node 1          | |        Node 2          | |        Node 3          |
|------------------------| |------------------------| |------------------------|
| node1.lab.local        | | node2.lab.local        | | node3.lab.local        |
|                        | |                        | |                        |
| RHEL                   | | RHEL                   | | RHEL                   |
|                        | |                        | |                        |
| Pacemaker              |<->| Pacemaker            |<->| Pacemaker              |
| Corosync               | | Corosync               | | Corosync               |
| PCS                    | | PCS                    | | PCS                    |
|                        | |                        | |                        |
| Application Resource   | | Standby Resource       | | Standby Resource       |
+-----------+------------+ +-----------+------------+ +-----------+------------+
            │                          │                          │
            │                          │                          │
            └──────────────────────────┼──────────────────────────┘
                                       │
                                       │
                              +------------------+
                              |  Shared Storage  |
                              |------------------|
                              | storage.lab.local|
                              |                  |
                              | iSCSI LUN         |
                              | /dev/sdb          |
                              | LVM / XFS         |
                              | /shared_data      |
                              +------------------+
```
