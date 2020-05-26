---
title: "Custom WinSCP ini files in Keypirinha"
date: 2020-05-26T15:14:27+08:00
draft: false
---

[Keypirinha](https://github.com/Keypirinha/Keypirinha/) is an awesome new keystroke launcher for Windows. Much better than Launchy or Keybreeze which I've used before. [^1]

The best part of Keypirinha for me is that the configuration is entirely driven by text files so it's really easy to tweak it to your satisfaction. The one annoyance I've had so far is with the WinSCP plugin for Keypirinha. It turns out that the package only works with the default locations for WinSCP settings - either the Registry entry or a WinSCP.ini file in %APPDATA%. [^2]

I have my WinSCP.ini file stored in a custom location so that I could manage it a dotfiles repository. The only workaround for now to get this working in Keypirinha is to create a symlink - ```cd %APPDATA% && mklink WinSCP.ini "X:\Path\to\WinSCP.ini"```

[^1]: Alfred is probably the one thing I miss from my macOS days but I suspect a lot of Alfred's capabilities came from things like AppleScript and were not specific to Alfred itself.
[^2]: Confirmed by the primary keypirinha developer here: <https://gitter.im/Keypirinha/Keypirinha?at=5a0ae582df09362e67086417>
