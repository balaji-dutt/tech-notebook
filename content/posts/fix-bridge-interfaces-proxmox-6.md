---
title: "Fix DHCP issues with Bridge interfaces in Proxmox 6"
date: 2020-06-30T12:15:55+08:00
draft: false
---

Another [Why does upgrading Linux break so many things post]({{< relref "frustrating-linux-tools.md" >}}).

This time with my Home-lab which consists of 3 NUC's [^1] running Proxmox. The machines have been running Proxmox 5 until recently, when the company announced that support for Proxmox 5 was ending. With quite a bit of trepidation, I upgraded to Proxmox 6 and yet again the same problem as before - this time with the networking stack where the servers stopped picking up the fixed IP's assigned to them from my router using MAC addresses. For some reason, following the upgrade to Proxmox 6 only the first bridge interface would pick up the MAC address of the underlying Ethernet interface. All the others would generate a new MAC address after every reboot which screwed things up. Even worse, trying to force a fixed IP in the `interfaces` file would seem to work, except the servers would have no connectivity on those interfaces!

I wound up wiping the servers completely and re-installing Proxmox 6 from scratch in an effort to minimize the variables, but even that did not help [^2]. It took me a number of attempts to finally figure out that you can force a MAC address for bridge interfaces using the following configuration:

```shell
auto vmbr1
iface vmbr1 inet dhcp
bridge_hw XX:XX:XX:XX:XX:XX #Specify Ethernet interface MAC address here
bridge-ports eth0.[XX] #Specify VLAN number here
bridge-stp off
bridge-fd 0
bridge-vlan-aware yes
bridge-vids 2-4094
bridge_maxwait 30 #Add this since the default is to wait 2 seconds which can be too short when you have multiple interfaces.
```

Once I added the `bridge_hw` configuration line, things start working normally again - until the next OS upgrade inevitably breaks something else I guess.

[^1]: One of which is actually a Aliexpress clone. Sadly this device is completely borked post upgrade and I had to switch it off completely.
[^2]: Although it did make the base configuration of each server much cleaner. A lot of software-cruft can build up in a machine over 3+ years of operation.
