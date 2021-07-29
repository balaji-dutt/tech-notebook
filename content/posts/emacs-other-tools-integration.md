---
title: "Setting up shellcheck, platinum-searcher and zeal with Emacs"
date: 2020-05-25T12:48:51+08:00
draft: false
---

Unlike the complexity of [setting up spellcheck](/posts/emacs-tools-integration-spellcheck), getting these tools to work with Emacs pretty much just requires installing them ```choco install shellcheck pt zeal```. There are few other things you can do to make your Emacs experience nicer though:

* In spacemacs, you can set "pt" as the default search tool by changing ```dotspacemacs-search-tools``` to have "pt" listed as the first tool - for example ```dotspacemacs-search-tools '("pt" "ag" "rg" "ack" "grep")```.
* You can limit the specific docset that zeal uses for a particular major-mode using the following configuration ```(add-to-list 'zeal-at-point-mode-alist '(nginx-mode . "nginx"))```
