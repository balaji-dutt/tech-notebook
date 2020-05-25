---
title: "Editing files with sudo in TRAMP-mode on Windows 10"
date: 2020-05-26T14:33:56+08:00
draft: true
---
If you want to edit files owned by other users (or root) in TRAMP, you should note the following:

* Make sure that the ```sudoers``` file includes ```/bin/sh``` in the list of allowed programs.
* You can then use the syntax ```/plinkx:[session-name]|sudo:[session-name]:/path/to/filename``` to access sudo protected files.
