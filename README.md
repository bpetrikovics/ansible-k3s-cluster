# Install a simple K3S cluster via Ansible

A simple Ansible playbook to install a single-master K3S cluster using the "official" installation script from Rancher.

I wrote and tested it on a cluster of Raspberry Pi 4's, using https://github.com/k3s-io/k3s-ansible as a starting point but simplifying it
as much as possible, e.g. delegating most of the installation process to the script (https://get.k3s.io). It's practically automating the
quick installation steps for a single-master cluster (later on I'll probably add support for multiple masters). The node token will be
extracted from the master server and used when installing each of the agents.

Please note it's specific to my environment and my needs, that is:

* Base OS config already done and SSH public key authentication configured
* All nodes run with netboot and NFS root, with a "central" NFS volume mounted as cluster shared storage. This share should already be prepared on the NFS server.
* An additional volume for /var/lib/rancher is mounted over iSCSI as containderd does not like to have its directory on NFS. The iSCSI discovery/login should be
previously performed and the device already be accessible as /dev/sda
* Kubeconfig will be downloaded to the ansible client host and copied into ~/.kube/config; it will also be patched to point to the correct, externally
visible IP address (instead of 127.0.0.1)

To use it, create an inventory file under inventory/ (use the provided sample file), fill it out with your corresponding hostnames, and then

```bash
ansible-playbook main.yml
```

This is my first ever Ansible playbook, so things might not be optimal or even correct. Suggestions are welcome.
