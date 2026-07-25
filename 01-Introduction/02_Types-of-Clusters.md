# Types of Clusters

Clusters are designed to meet different business requirements. Some focus on ensuring continuous service availability, while others improve performance, distribute workloads, or provide shared storage. Choosing the right cluster type depends on the application's requirements.

---

## Types of Clusters

1. High Availability (HA) Cluster
2. Load Balancing Cluster
3. High Performance Computing (HPC) Cluster
4. Storage Cluster
5. Database Cluster
6. Kubernetes Cluster

---

# 1. High Availability (HA) Cluster

## Purpose

A High Availability (HA) Cluster ensures that applications remain available even if one server fails. It eliminates a single point of failure by automatically moving services to another healthy node.

### Architecture

                 Client
                   │
            Virtual IP (VIP)
                   │
        +---------------------+
        |   Pacemaker Cluster |
        +---------------------+
             │           │
      +-------------+ +-------------+
      |    Node1    | |    Node2    |
      |   Active    | |   Standby   |
      +-------------+ +-------------+


### Characteristics

- Automatic Failover
- High Availability
- Minimal Downtime
- Used for Critical Applications

### Examples

- Red Hat Cluster (Pacemaker + Corosync)
- Windows Failover Cluster

### Common Use Cases

- Banking
- ERP Applications
- Healthcare Systems
- Financial Services

---

# 2. Load Balancing Cluster

## Purpose

A Load Balancing Cluster distributes incoming client requests across multiple servers to improve performance and prevent any single server from becoming overloaded.

### Architecture

                  Users
                    │
            +----------------+
            | Load Balancer  |
            +----------------+
              │      │      │
              ▼      ▼      ▼
         +------+ +------+ +------+
         |Node1 | |Node2 | |Node3 |
         +------+ +------+ +------+

### Characteristics

- Better Performance
- Traffic Distribution
- Increased Scalability
- High Throughput

### Examples

- HAProxy
- NGINX
- F5 BIG-IP

### Common Use Cases

- Web Servers
- E-commerce Websites
- APIs
- Cloud Applications

---

# 3. High Performance Computing (HPC) Cluster

## Purpose

An HPC Cluster combines the computing power of multiple servers to perform complex calculations much faster than a single server.

### Architecture

          Job Scheduler
                │
      -----------------------
      │     │      │      │
   Compute Compute Compute Compute
    Node1   Node2   Node3   Node4


### Characteristics

- Parallel Processing
- Massive Computing Power
- Faster Execution
- Scientific Computing

### Examples

- Research Labs
- Weather Forecasting
- AI & Machine Learning
- Simulations

---

# 4. Storage Cluster

## Purpose

A Storage Cluster provides centralized, highly available storage that can be accessed by multiple servers.

### Architecture

            Applications
                 │
      ------------------------
      │                      │
   Server1               Server2
      │                      │
      └──────────┬───────────┘
                 │
          Shared Storage

### Characteristics

- Centralized Storage
- Data Replication
- High Availability
- Data Protection

### Examples

- GlusterFS
- Ceph
- VMware vSAN

### Common Use Cases

- File Servers
- Virtualization
- Backup Storage

---

# 5. Database Cluster

## Purpose

A Database Cluster provides high availability and scalability for databases by replicating data across multiple database servers.

### Architecture

             Application
                  │
         +-----------------+
         | Database Cluster|
         +-----------------+
            │          │
      +-----------+ +-----------+
      | Primary   | | Secondary |
      +-----------+ +-----------+


### Characteristics

- Database Replication
- Automatic Failover
- High Availability
- Data Consistency

### Examples

- MySQL Cluster
- PostgreSQL Cluster
- Oracle RAC

### Common Use Cases

- Banking Systems
- ERP Databases
- E-commerce Platforms

---

# 6. Kubernetes Cluster

## Purpose

A Kubernetes Cluster manages containerized applications by automatically deploying, scaling, and recovering containers.

### Architecture

               Users
                 │
          Kubernetes API
                 │
          +--------------+
          | Master Node  |
          +--------------+
            │     │     │
      +---------+ +---------+ +---------+
      | Worker1 | | Worker2 | | Worker3 |
      +---------+ +---------+ +---------+

### Characteristics

- Container Orchestration
- Auto Scaling
- Self Healing
- Rolling Updates

### Examples

- Kubernetes
- OpenShift

### Common Use Cases

- Microservices
- Cloud-native Applications
- DevOps

---

# Summary

Each cluster type is designed to solve a specific problem. High Availability clusters ensure continuous service, Load Balancing clusters improve performance, HPC clusters increase computational power, Storage clusters provide resilient shared storage, Database clusters protect critical data, and Kubernetes clusters simplify container management.

**In this repository, the primary focus is on **High Availability (HA) Cluster** using **Red Hat Enterprise Linux**, **Pacemaker**, and **Corosyn**c**.
