
***If Resource Groups already exist...***

Why did the Pacemaker developers create Constraints?

Suppose we have:

#### WebGroup

├── SharedFS

├── VirtualVIP

└── Apache

Pacemaker automatically knows:

Start Order: SharedFS -> VIP -> Apache

Stop Order: Apache -> VIP -> SharedFS

Suppose a company has:

Apache

MySQL

NFS

HAProxy

Tomcat

FTP

Should all of them be in one Resource Group? Absolutely not.

Imagine: WebGroup -> Apache -> VIP -> Filesystem -> MySQL -> Tomcat -> NFS -> FTP -> Mail

That would be a disaster.

If Apache fails,

everything moves.

We don't want that.

### Real Production Example

                 Clients

                    │

              Load Balancer

           ┌────────┴────────┐

           │                 │

        Web Server       Web Server

           │

        MySQL Database

           │

      Shared Storage

***Apache needs***

VIP

SharedFS

But MySQL is independent. It should not move because Apache restarted. Resource Groups are too coarse for this.

# Constraint

A **Constraint** is a scheduling rule that tells Pacemaker where a resource should run, with which other resources it should run, and in what order resources should start or stop.

### Constraint of three types

1. **Location Constraint**

2. **Colocation Constraint**

3. **Ordering Constraint**
