## Resource Group

A **Resource Group** is a collection of resources that Pacemaker treats as a single unit.

Pacemaker manages:

WebGroup

├── SharedFS

├── VirtualVIP

└── Apache

***How Pacemaker Internally Sees a Group***

Resource Group

↓

Resource 1

↓

Resource 2

↓

Resource 3

#### Create a Resource Group

pcs resource group add WebGroup SharedFS VirtualVIP WebServer

Verify:

pcs resource config

pcs status

***Remove a Resource from Group***

pcs resource group remove WebGroup WebServer

***To Delete a Group***

pcs resource group delete WebGroup

Resources still exist. Only the group is removed.

***Interviewer***

"What happens if one resource in a group fails?"

Pacemaker first attempts recovery according to the resource's failure policy. If the resource cannot be recovered on the current node, Pacemaker typically moves the entire group to another eligible node so that the application stack remains together.

#### These two commands are not the same:

pcs resource group add WebGroup SharedFS VirtualVIP WebServer

vs.

pcs resource group add WebGroup WebServer VirtualVIP SharedFS

The first one starts the filesystem before Apache, which is what we want. The second one is incorrect for this application because Apache would start before the filesystem is available.
