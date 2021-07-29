---
title: "Setting up spellcheck in Emacs"
date: 2020-05-25T11:26:57+08:00
draft: false
---

Since Emacs is born in GNU-land, like all other Unix(y) tools it relies on other programs to do specific things. This includes some basic capabilities such as spell-checking, but the same applies for other integrations as well. All of this is of course much less straightforward when it comes to working in Windows.

* Install hunspell using chocolatey[^1] ```choco install hunspell.portable```
* Download the spelling dictionaries from the [Libreoffice website](https://extensions.libreoffice.org/extensions/english-dictionaries/2019-11.01)
  * The location of these dictionary files changes regularly so the above link can break. For reference, the files you need are the "English dictionaries for OpenOffice/LibreOffice"
  * The file might come in ```.oxt``` format, but it's really just a zip file. Change the file extension to ".zip" and you can extract the contents.
* Place the contents of the dictionary files in ```%ChocolateyInstall%\lib\hunspell.portable\tools\dict``` You might need to create the "tools\dict" folders the first time.
* Create a new ```DICPATH``` environment variable pointing to the above location.
* Add the following snippet to your Emacs init.el

```emacs
  (with-eval-after-load "ispell"
    (setq ispell-dictionary-alist
          '((nil "[A-Za-z]" "[^A-Za-z]" "[']" t
                 ("-d" "en_GB" "-i" "utf-8") nil utf-8)
            ("american"
             "[A-Za-z]" "[^A-Za-z]" "[']" nil
             ("-d" "en_US") nil utf-8)
            ("english"
             "[A-Za-z]" "[^A-Za-z]" "[']" nil
             ("-d" "en_GB") nil utf-8)
            ("british"
             "[A-Za-z]" "[^A-Za-z]" "[']" nil
             ("-d" "en_GB") nil utf-8)))
    (progn
      (setq ispell-dictionary "english"
            ispell-extra-args '("-a" "-i" "utf-8")
            ispell-silently-savep t
            ))
    (add-to-list 'ispell-skip-region-alist '("^#+BEGIN_SRC" . "^#+END_SRC"))
    )
```

* This configuration sets up the different English spelling variants, assigns "english" as the default dictionary language (mapped to en-GB) and skips source code snippets in orgmode files.
* To enable spellcheck in a particular Emacs major-mode, you need to do the following:

```emacs
    ;; Replace org-mode-hook with the appropriate mode-hook that you are interested in.
    (add-hook 'org-mode-hook
              (lambda ()
                (setq flyspell-mode 1)))

```

[^1]: According to HNN, real hackers would use ```scoop```. I honestly did not know of ```scoop``` so I used chocolatey. I've looked at the different scoop repositories and could not find one of the tools I use in the popular repositories. So instead of relying on some sketchy GitHub repo, I'm happy to rely on one central website that has sketchy Powershell scripts.
