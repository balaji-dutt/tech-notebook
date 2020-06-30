---
title: "Fix Intel Network Adapter (E1000) hanging/errors in Proxmox"
date: 2020-06-30T13:03:05+08:00
draft: false
---

After tearing my hair out [^1] with [the DHCP issue]({{< relref "fix-bridge-interfaces-proxmox-6.md" >}}), the next problem I started seeing was one my NUC's would randomly hard crash and only powering it down and back up again would fix the issue. In most cases, the syslog would be completely empty which made things worse but I eventually managed to find an entry in the syslog that looked like this:

```shell
e1000e 0000:00:1f.6 eno1: Detected Hardware Unit Hang
```

Checking on the Proxmox forums for that error message turned up [this very long thread](https://forum.proxmox.com/threads/e1000-driver-hang.58284/) within which I found the answer. This issue apparently affects most Intel network adapters and although the thread claims it is fixed as of Kernel version 5.3, I was still seeing this error in a fresh install. The solution is to first install ethtool and then add the following line to your `interfaces` file:

``` shell
ethtool -K eno1 gso off tso off rxvlan off txvlan off
#You may also want to try adding gro off tx off rx off sg off to the above
```

Apparently this can cause an increase in CPU utilization on the server, but the impact if any seems to be minimal. But yet again, completely baffled by why an OS upgrade is breaking a bog-standard hardware/software configuration.

[^1]: Very metaphorical I assure you since I've been rocking the cue-ball look for years now 😜
