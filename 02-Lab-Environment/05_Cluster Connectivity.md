# Verify Cluster Connectivity

Before installing Pacemaker and Corosync, every node in the cluster must be able to communicate reliably with the other nodes.
These verification steps help ensure that the operating system, networking, and security settings are correctly configured.

## Verify Machine ID

Run the following command on every cluster node.

cat /etc/machine-id

Example output:

fbeea06fd5d74c20b42083cb7d3d78b1

If two nodes have the same Machine ID, remove the existing file and generate a new one.

sudo rm -f /etc/machine-id

sudo systemd-machine-id-setup

sudo reboot

After the reboot, verify the new Machine ID again.

## Configure /etc/hosts

Open the hosts file on every node.

sudo vi /etc/hosts

configuration

192.168.43.128    node1.lab.local    node1

192.168.43.129    node2.lab.local    node2

192.168.43.130    node3.lab.local    node3

Save the file and repeat the same configuration on all cluster nodes.

## Test Internet Connectivity

Test connectivity by pinging a public domain.

ping -c 4 google.com

## Test Name Resolution

After configuring the /etc/hosts file, verify that each node can resolve the hostname of the other node.

Test from Node1

ping node2

Test from Node2

ping node1

Test from Node3

ping node1 or node2

## Test Cluster Network

Test from Node1

ping 192.168.43.129

Test from Node2

ping 192.168.43.128

Expected latency

time=0.3 ms

The latency should typically be less than 1 ms in a virtual lab environment.

## Test SSH Connectivity

From Node1

ssh redhat@node2

From Node2

ssh redhat@node1

From Node3

ssh redhat@node2

The login should succeed without prompting for a password.

## Verify Firewall Configuration

Check Firewall Status

systemctl status firewalld

For a lab, you may temporarily stop the firewall.

sudo systemctl stop firewalld

sudo systemctl disable firewalld

#### Note: In production environments, do not disable the firewall. Instead, configure the required firewall rules for Pacemaker and Corosync.

## Verify SELinux Status

getenforce

Enforcing

## Synchronize Time

Install Chrony

sudo dnf install -y chrony

sudo systemctl enable --now chronyd

chronyc tracking

chronyc sources -v

## Verification Checklist

Before proceeding with the installation of Pacemaker and Corosync, verify that the following requirements have been completed successfully.

Unique Machine ID on every node	✅

/etc/hosts configured correctly	✅

Internet connectivity verified	✅

Hostname resolution working	✅

Private cluster network reachable	✅

SSH connectivity testing	✅

Firewall configuration verified	✅

SELinux status verified	✅

Time synchronization completed	✅
