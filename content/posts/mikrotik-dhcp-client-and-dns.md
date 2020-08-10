---
title: "Using Custom DNS Servers with Mikrotik Routers"
date: 2020-08-10T15:23:08+08:00
draft: false
---

The most important reason for not using the DNS servers that your ISP provides is protecting your privacy as it's been proved many times that ISP's are not averse to hijacking your DNS results to serve ads or worse. But the other reason is that quite often the DNS servers that ISP's operate are _slow_, they tend to have long cache times which means that changes to your domain IP's can be hard to test from your local machine.

I had switched to using the [Quad9 DNS service](https://www.quad9.net/) but until recently I would find whenever I made changes to domain IP's etc. running a `nslookup domain.tld 9.9.9.9` would show an updated IP but a plain `nslookup domain.tld` (which would use the DNS server specified in my LAN settings) would show the old IP's even if I flushed the DNS cache locally and on the router. It was only when researching something else related to Mikrotik routers, I came across a forum post that pointed out something very important - the "Peer DNS" settings in Mikrotik's DHCP client *overrides* any DNS servers configured under `IP>DNS`. In other words, if in the screenshot below the "Peer DNS" option is checked:

![peer-dns](/img/mikrotik-peer-dns.png)

Then it's not going to matter what you have configured anywhere else. And guess what option[^1] I had not unchecked until recently? _D-oh!_

Tested with: RouterOS 6.46 on Mikrotik RB750Gr3

[^1]: The "Peer NTP" setting is also useful if you intend to configure custom NTP servers.
