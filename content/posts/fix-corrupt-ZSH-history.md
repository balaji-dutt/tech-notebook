---
title: "Fix Corrupt ZSH History File"
date: 2020-07-19T17:47:58+08:00
draft: false
---

I noticed that one of my homelab machines had a slight clock drift problem. However, once I fixed that I wound up with an issue where ZSH starting printing a message "Corrupt history file" after every command. I suspect that this is because the History file had a timestamp in the future as compared to the system time. Since I did not want to lose my entire shell history, I was able to fix this by running the following commands:

```shell
mv .zsh_history .zsh_history_bad
strings .zsh_history_bad > .zsh_history
fc -R .zsh_history
```

Note that you might need to wait until the system time is _after_ the last timestamp in your history file before running these commands - else you will get the "Corrupt history file" error again.

Tested with: ZSH 5.7.1 on Debian Buster
