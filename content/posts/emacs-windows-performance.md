---
title: "Improving Emacs startup performance in Windows"
date: 2020-05-10T16:55:29+08:00
draft: true
---

Much like everything else to do with Emacs, getting it to run "snappily" [^1] is a bit of a black art especially Windows. Here's a quick list of everything that I did to get Emacs to start-up quickly.

* Pick the right flavour of Emacs
    - The default Windows binary available on [the GNU website](https://www.gnu.org/software/emacs/download.html#windows) is not the most optimized version and lacks some quality of life features such as having GnuTLS built in, which is needed for getting MELPA packages.
    - A good alternative is [emax64](https://github.com/m-parashar/emax64), which has a bunch of optimizations and libraries baked in. The only problem that I have with it is that it is only available as tarball releases which means you need to keep track of when a new version comes out.
    - The other alternative is [msys2](https://www.msys2.org/) which has the benefit of coming with a package manager to handle updates. This is an optimized version as well but it's still missing some additional libraries which are needed to get the best experience with Emacs distributions like spacemacs. I wound up installing the following additional packages:

        ```pacman -S  mingw-w64-x86_64-giflib mingw-w64-x86_64-libjpeg-turbo mingw-w64-x86_64-libpng mingw-w64-x86_64-librsvg mingw-w64-x86_64-libtiff mingw-w64-x86_64-imagemagick mingw-w64-x86_64-libxml2```


* Exclude Emacs folders and paths from Antivirus scans - I cannot *emphasize* how much of a difference this makes! Even Windows Defender which is considered the least offensive in terms of performance hits from an AV scanner added nearly 10 seconds to the startup time. And if you are using Acronis True Image for backups, disable the Ransomware Protection. Even excluding Emacs paths in the Acronis program is not enough, and leaving it on added nearly _90 seconds_ to Emacs startup times 😲.

* Improve org-mode startup times: Org-mode is amazing. Org-mode is also slow. With [spacemacs](https://www.spacemacs.org/), the best way I found to improve org-mode performance is to set ```dotspacemacs-scratch-mode``` to ```'org-mode``` which does slow down startup times but makes loading org-mode files later much easier.

* Run Emacs daemon at startup using ```runemacs.exe --daemon```. This will take care of getting the slow startup out of the way and makes loading the actual editor much faster.

In general, once you have got your Emacs ```init.el``` configured to your liking (is there such a thing as an ```init.el``` that's actually done?) the Emacs daemon at startup takes care of most things. But while you are configuring & reconfiguring your ```init.el``` at the start, every second you can save on an Emacs restart is precious.

[^1]: Snappiness is relative here, this is _Emacs_ we are talking about. The best one-liner I saw about Emacs was "Emacs is an IDE that happens to support editing text files."
