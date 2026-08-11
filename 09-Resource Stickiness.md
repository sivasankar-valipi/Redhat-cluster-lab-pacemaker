# Resource Stickiness

Understand how Pacemaker decides whether a resource should remain on its current node or move to another available node.

**Resource Stickiness** helps prevent unnecessary resource movement and resource thrashing after a node recovers.

---

## What is Resource Stickiness?

Resource Stickiness is a score assigned to a resource that tells Pacemaker. Pacemaker considers this score along with other placement scores and constraints when deciding where a resource should run.

---

## Why Do We Need Resource Stickiness?

Consider the following scenario:

```text
node3
  |
  ├── SharedFS
  ├── ClusterIP
  └── Apache

If node3 fails, Pacemaker moves the application to another node:

node3
  ↓
Failure
  ↓
node1
  |
  ├── SharedFS
  ├── ClusterIP
  └── Apache

When node3 recovers, Pacemaker could potentially move the application back.

This unnecessary movement can cause:

Application interruption

Client reconnections

Cache rebuilding

Additional storage operations

Resource thrashing

Resource Stickiness helps prevent unnecessary failback.

Resource Stickiness and Location Constraints are two concepts are different.

#### Location Constraint

Where should the resource preferably run?

Example:

node1 = 300

node2 = 200

node3 = 100

#### Resource Stickiness

Should the resource remain on its current node?

Example:

Current node = node3

Stickiness = 250

Pacemaker considers both when making its placement decision.

Assume:

Location Scores:

node1 = 300
node2 = 200
node3 = 100

The resource is currently running on node3.

Resource stickiness:

250

Pacemaker can consider the current-node score as:

node1 = 300

node2 = 200

node3 = 100 + 250
       = 350

Therefore:

node3 = 350

node1 = 300

node2 = 200

The resource remains on node3 because its effective score is higher.

#### Resource Thrashing

Without appropriate stickiness, a resource may repeatedly move between nodes when nodes repeatedly fail and recover.

Example:

node3
  ↓
Failure
  ↓
node1
  ↓
node3 recovers
  ↓
node3
  ↓
node3 fails again
  ↓
node1

This repeated movement is called resource thrashing.

Resource Stickiness helps Pacemaker avoid unnecessary movement when the current node is healthy.

### A resource stickiness value can be configured using:

pcs resource defaults update resource-stickiness=<VALUE>

pcs resource defaults update resource-stickiness=200

Understanding Stickiness Values

Stickiness = 0

**resource-stickiness=0**

Resource Stickiness is a scheduling score that makes Pacemaker prefer keeping a resource on its current node, helping prevent unnecessary resource movement and resource thrashing.

### Does Resource Stickiness prevent failover?

No. If the current node fails or becomes unsuitable, Pacemaker moves the resource to another eligible node.

### What is the difference between Location Constraint and Resource Stickiness?

Location Constraint:

Where should the resource run?

Resource Stickiness:

Should the resource stay where it is?

### Why is Resource Stickiness useful?

It prevents unnecessary failback and reduces resource movement after a recovered node becomes available again.

### What happens if a node hosting a sticky resource fails?

The stickiness cannot keep the resource on an unavailable node. Pacemaker moves the resource to another eligible node.

Location says where Pacemaker prefers to place a resource. Stickiness tells Pacemaker how strongly the resource prefers to remain where it is already running.
