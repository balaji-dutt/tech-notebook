---
title: "Dealing with tmux/sslh and Linux upgrades"
date: 2020-05-25T13:13:34+08:00
draft: false
---

I understand that breaking changes are required from time in time in software. But I really don't get why maintainers of some software tools are so obstinate about not wanting to write code to manage such scenarios. Case in point - tmux in Debian "buster" and at some point "sslh" as well.

I use tmux when setting up new servers. Once the servers are setup, I might leave a tmux session open with tools like "htop"/"iftop" etc. running so I can generally see how the servers are behaving. But apart from this, I don't use tmux as extensively as some folks do - which means I cargo-culted together a working ```.tmux.conf``` back in 2013 and have pretty much used that configuration unchanged since then. Turns out though that as far back as February 2014 some of my configuration directives were deprecated but in all the version upgrades since there hasn't been a single warning message printed anywhere. That is until yesteday, when I upgraded a bunch of servers from Debian "Stretch" to Debian "Buster".

The upgrade itself went relatively smoothly (which is always a bit nerve-racking) and initially everything seemed to be working. Lulled into a sense of over-confidence, I enabled "buster-backports" which installed tmux-2.9 and then all those "deprecated" configurations were now "unsupported" and I was suddenly left with a half-working tmux setup. Cue the frantic GitHub searches which lead me to [this GitHub issue](https://github.com/tmux/tmux/issues/1689) and I'm amazed that the response from the Developer is "Well you should read the change-logs".

Here's an idea - why not print a message in the program on startup? Postfix does it, and Postfix deals with way more gnarly code than tmux ever will! It took a couple of hours to get this sorted out which wasn't helped by the fact that as far as I can tell the [tmux documentation](https://man.openbsd.org/tmux.1#STYLES) is just wrong in parts - ```fg=colorNNN``` is not supported for some attributes despite what the documentation says.

Another package that I've previously had this wonderful experience with is "sslh" which I use to multiplex traffic on Port 443 for my homelab. Very much *not* looking forward to upgrading the NUC-clone that I use as the jumphost for my homelab to Debian "buster" 😓
