## Components

### Client

The client is the end user or application that accesses the service using a **Virtual IP (VIP)** instead of connecting directly to a specific server.

---

### Virtual IP (VIP)

A Virtual IP is a floating IP address managed by Pacemaker.

- Always points to the active node
- Automatically moves during failover
- No client-side configuration changes required

---

### Node

A node is an individual RHEL server participating in the cluster.

Each node contains:

- Red Hat Enterprise Linux
- Pacemaker
- Corosync
- PCS

One node is active, while another can be in standby depending on the cluster configuration.

---

### Pacemaker

Pacemaker is the **Cluster Resource Manager**.

Responsibilities:

- Starts resources
- Stops resources
- Monitors resources
- Detects failures
- Performs automatic failover
- Maintains resource constraints

---

### Corosync

Corosync is responsible for communication between nodes.

Responsibilities:

- Heartbeat communication
- Membership management
- Quorum calculation
- Cluster messaging

---

### PCS

PCS (Pacemaker Configuration System) is the command-line tool used to configure and manage the cluster.

Example commands:

pcs status
pcs resource show
pcs cluster start --all
pcs node status

---

### Cluster Resource

A resource is any service managed by Pacemaker.

Examples:

- Apache HTTP Server
- NFS
- Database
- Virtual IP
- Custom Application

---

### Heartbeat

Corosync continuously exchanges heartbeat messages between nodes.

If a heartbeat is not received within the configured timeout, Pacemaker assumes the node has failed and initiates failover.

---
