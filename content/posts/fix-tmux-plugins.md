---
title: "Fix tmux plugins not loading"
date: 2020-07-11T15:50:02+08:00
draft: false
---

Continuing the theme of [upgrades breaking things]({{< relref "frustrating-linux-tools.md" >}}), I realized that after the reinstallation of [Proxmox]({< "fix-bridge-interfaces-proxmox-6.md" >}}) I was not able to restore my tmux sessions. The mapped commands in my `.tmux.conf` for session-save, session-restore were not working but everything else was.

An initial check seemed to suggest it was PEBKAC error since I needed to clone the [tmux plugin manager](https://github.com/tmux-plugins/tpm) into my .tmux folder. However, even after doing that and trying to install the plugins via the plugin manager the session functionality was not working. I decided to check the GitHub issues in the tpm repo and in a completely unsurprising turn of events, there's [an open issue](https://github.com/tmux-plugins/tpm/issues/97) that since tmux 2.2 this has been broken. Anybody wanna take a bet that there was some barely announced deprecation of a tmux feature that's the root cause?

Anyway, I was able to fix the plugins not loading by following the process below:
* Clone the plugin repo into your tpm plugins directory. For example to install `tmux-sensible` run `git clone https://github.com/tmux-plugins/tmux-sensible ~/.tmux/plugins/tmux-sensible`
* Add the line `run '~/.tmux/plugins/tmux-sensible/sensible.tmux'` to the end of your `.tmux.conf` file.
* Either re-start your tmux session or run `tmux source ~/.tmux.conf`

Tested with: tmux-3.1b on Debian "Buster"
