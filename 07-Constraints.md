
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

A **Location Constraint** tells pacemaker where a resource should or should not run by assigning scores to nodes.

2. **Colocation Constraint**

A **Colocation Constraint** tells Pacemaker that one resource should run on the same node as another resource. It is used when resources depend on each other but are managed independently.

3. **Ordering Constraint**

An **Ordering Constraint** tells Pacemaker the sequence in which resources should start or stop. It ensures that dependent resources are available before applications that rely on them.

## Location Constraint:

#### Syntax

pcs resource location RESOURCE prefers node=SCORE

pcs resource location WebGroup prefers node1=100

#### Pacemaker understands values like:

200

100

50

0

-50

-100

INFINITY

-INFINITY

***Positive numbers - I like this node***

***Negative numbers - I don't prefer this node***

***INFINITY - Always Prefer***

***-INFINITY - Never Prefer***


## Colocation Constraint

#### Syntax

pcs resource colocation add RESOURCE_A with RESOURCE_B

pcs resource colocation add VirtualIP with Webserver

***Use Resource Groups when resources are tightly coupled**

***Use Colocation Constraints when resources belong to different groups or independent applications but still need to run together***

## Ordering Constraint

#### Syntax

pcs constraint order start RESOURCE1 then start RESOURCE2

pcs constraint order start SharedFS then Webserver

pcs constraint order start VirtualIP then Webserver

