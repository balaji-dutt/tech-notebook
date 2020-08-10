---
title: "Using NTP Server Names instead of IP's in Mikrotik Routers"
date: 2020-08-10T15:52:17+08:00
draft: false
---

I've been having some issues with dying equipment in my kitchen recently which has resulted in the breakers tripping in the electrical junction box a few times. That meant that all of my network and servers went down unexpectedly which created it's own problems - as it turns out I have enough equipment plugged in that just flipping the breaker again would result in another trip, due to too much power draw!

In any case, due to the unexpected shutdowns I suddenly noticed that the timestamps on my router were way off from the local time on my local PC. I was initially puzzled by this, since I remembered setting up the SNTP client on my Mikrotik router but sure enough a quick check showed that the IP's I had specified in the client were not working. Now this isn't unexpected - I was using the pool.ntp.org servers and they specifically recommend *against* using bare IP's. The problem was that when I configured the router, it wouldn't let me specify a server name in the configuration (or so I thought).

Turns out that Mikrotik does allow you to specify a server name instead of IP for the SNTP client, but like so many other things Mikrotik figuring out how to do this is not straightforward and their documentation is of no help. So here's a screenshot of the appropriate settings under `System>SNTP Client`:

![SNTP Client](/img/mikrotik-sntp.png)

The key things to note here are:

1. You have to put an IP in the Primary NTP/Secondary NTP fields as they are mandatory. However `0.0.0.0` is perfectly acceptable.
2. You can add Server Names by clicking on the "Add value" UI button next to "Server DNS Names"
3. Notice how Dynamic Servers is empty? That's because the "Peer NTP" option under `IP>DHCP Client` is unchecked. For more info about these Peer settings, look at my [earlier post]({{< relref "mikrotik-dhcp-client-and-dns.md" >}})

Once you configure set it up this way, your Mikrotik device will automatically sync with the upstream NTP Servers every 15 minutes and best of all, you are being a good user by not connecting to bare IP's of the ntp.pool.org servers.
