---
title: "Making Putty, Windows 10 SSH & WSL work together"
date: 2020-05-12T17:58:49+08:00
draft: false
---

Since PuTTY has been the defacto SSH client on Windows since Windows XP, I tend to use ```PAGEANT.EXE``` to manage my SSH keys. Now that ```PUTTYGEN.EXE``` supports more robust ed25519 keys, I can get good security for my SSH keys and integration with apps such as Git and Emacs.

The only annoyance has been the Windows 10 ```ssh.exe``` and WSL1. Typically to get SSH keys working in these environments, you needed to setup keys again with ```ssh-agent``` or worse set it up twice - once for ```ssh.exe``` and again for WSL1.

Enter [wsl-ssh-pageant](https://github.com/benpye/wsl-ssh-pageant). This tool allows me to load my keys in ```PAGEANT.EXE``` only and then have them be automagically visible in ```ssh.exe``` and WSL1. Here's how to get it working:

- Download the binary from the [releases page](https://github.com/benpye/wsl-ssh-pageant/releases). Unzip and add it to a folder in your ```%PATH%``` somewhere.
- Create an Windows Environment variable ```SSH_AUTH_SOCK``` and set the value as ```\\.\pipe\ssh-pageant```
- Create a folder ```.wsl-ssh-pageant``` in your ```%USERPROFILE%``` directory
- Add the following line to your ```.bashrc``` in your ```%USERPROFILE``` directory as well as your ```$HOME``` folder in WSL1:

    ```export SSH_AUTH_SOCK=/mnt/c/Users/USERNAME/.wsl-ssh-pageant/ssh-agent.sock```

- Add the following line to a batch file and include the batch file in your startup folder

    ```start wsl-ssh-pageant-amd64-gui.exe --force --systray --winssh ssh-pageant --wsl C:\Users\USERNAME\.wsl-ssh-pageant\ssh-agent.sock```

Now launch ```ssh.exe``` or ```bash.exe``` and type ```ssh-add -l```. Rejoice!

One gotcha that had me stuck for a while though - in your SSH config file, make sure to comment out any ```IdentitiesOnly yes``` directives. This can potentially be a problem if you have a lot of SSH keys in ```PAGEANT.EXE```[^1] but YMMV.

At the moment, wsl-ssh-pageant does not support WSL2 directly although there is an open issue in the GitHub repo about this.

[^1]: Yes, the use of Caps is deliberate.
