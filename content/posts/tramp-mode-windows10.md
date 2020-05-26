---
title: "Using TRAMP-mode in Emacs on Windows 10"
date: 2020-05-26T14:09:56+08:00
draft: false
---
TRAMP is another one of those Emacs power tools that once you have it working you go "How did I live without this till now?" [^1].

Getting it working on Windows 10 (I'm sensing a theme here) can be tricky due to some gotchas.

* A lot of articles on TRAMP will suggest using ```/ssh``` to start the session. **Do not** use this with Windows 10! Annoyingly, TRAMP will parse the SSH config file on Windows 10 but any attempt to connect using those sessions will just hang Emacs.
* Windows specific guides on TRAMP will suggest using ```/plink```. This works - but only if you like re-entering the host address again & again.
* The most reliable way to use TRAMP on Windows 10 is to use ```/plinkx```. This will show a list of saved ```PUTTY``` sessions and will prompt for password entry if needed (or use your SSH keys loaded in ```PAGEANT.EXE```).

[^1]: A rough equivalent is available for Sublime Text with rsub and now for VSCode with Remote Development. VSCode probably comes closest to TRAMP in that it does not require additional tooling to be available on the remote server but it seems to require you to re-add your host configuration to the extension in VSCode, unlike TRAMP.
