We configured the Virtual IP and Apache as Pacemaker resources.

If Node1 fails and Apache starts on Node2, Node2 must have the same website content. If both nodes use their own local disks, the web pages may be different or may not exist at all.

To solve this problem, both nodes must access the same shared storage. Pacemaker manages this shared storage using a **Filesystem Resource**.

## Filesystem Resource

A **Filesystem Resource** is a Pacemaker-managed resource that mounts and unmounts a filesystem on the appropriate cluster node.

### Why Do We Need a Filesystem Resource?

Consider a shared iSCSI disk.

         ```text
                 iSCSI Target
                      │
                      ▼
            Shared Disk (/dev/sda)
                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
          ┌───────┐         ┌───────┐
          │ node1 │         │ node2 │
          └───────┘         └───────┘
```

Although both nodes can discover the disk, only one node should mount it at a time (for a standard filesystem like XFS or ext4).

If both nodes mount the same filesystem simultaneously without a cluster-aware filesystem, data corruption can occur.

****Pacemaker prevents this by ensuring that****

The filesystem is mounted on only one node.

It is cleanly unmounted before failover.

It is mounted on the new active node after failover.

**Resource Agent**

The Filesystem Resource uses the following Resource Agent:

****ocf:heartbeat:Filesystem****

