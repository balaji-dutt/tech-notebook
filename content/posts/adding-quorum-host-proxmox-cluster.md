---
title: "Adding a Quorum-only Host to Proxmox Cluster"
date: 2020-07-19T17:25:44+08:00
draft: false
---


I've mentioned this in passing in an [earlier post about Proxmox]({{< relref "fix-bridge-interfaces-proxmox-6.md" >}}) but until recently, I had a 3-node Proxmox cluster on a set of NUC's for my Homelab. The 3-node setup is great because it means you can reboot individual nodes without causing the entire cluster to become unavailable. Unfortunately, as I mentioned in the earlier post about Proxmox the NUC-clone I got from Aliexpress was basically unusable after the upgrade to Proxmox 6 - it would have regular Kernel panics and even installing kdump or setting up remote logging did not yield any results. What I did discover is that it worked fine with stock Debian Buster, so it's something in the more recent 5.x kernel series that's causing the kernel panics. But the result of that issue was that I was down to a 2-node cluster, which became a problem since any time I rebooted one node the other node would either stop all VM's from running or worse, crash.

I remembered reading that Proxmox did offer a way to overcome the limitations of a 2-node cluster and some searching eventually turned up a link to [this Proxmox documentation article](https://pve.proxmox.com/pve-docs/chapter-pvecm.html#_corosync_external_vote_support). While the steps listed there seem straightforward, there are a few additional things that need to be done to add a regular Debian node as a Quorum node. In particular:

1. Add an entry to the `hosts` file on the target machine for the IP address that it will be listening to for Quorum requests. In other words, if the external IP that will be added to the Proxmox cluster is `192.168.0.10` then add `192.168.0.10 foo` to the machine's `hosts` file.
2. Add entries to the target machine's `hosts` file for the other 2 nodes in the Proxmox cluster.
3. Make sure that you allow root login via SSH on the target machine. This can be configured as `PermitRootLogin without-password` in `sshd_config`.
4. If you do the above, copy the SSH private key for the root user on the target machine to the Proxmox nodes. Then make sure you can actually login via SSH from the proxmox nodes to the target machine.
5. You might need to temporarily set `PermitRootLogin yes` (to allow password based logins) on the target machine's `sshd_config`.

Once you have done these steps the steps mentioned in the Proxmox documentation should work correctly and you can get the equivalent of a 3-node Proxmox cluster.
