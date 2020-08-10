---
title: "Generating pandoc-style references from Bibliography files in Emacs"
date: 2020-06-30T10:50:26+08:00
draft: false
---

As I mentioned briefly in [my earlier post on using Pandoc]({{< relref "pandoc-markdown-citations.md" >}}), one way to be able to use citations with pandoc is to use the zotxt extension with Zotero. The zotxt extension comes with an emacs integration as well [zotxt-emacs](https://github.com/egh/zotxt-emacs), but I could not get it to work reliably.

Instead after quite a bit of searching on GitHub, I came across [this gist](https://gist.github.com/kleinschmidt/5ab0d3c423a7ee013a2c01b3919b009a) which works pretty reliably. Reproducing the Gist here for reference:

```lisp
(setq reftex-default-bibliography '("/path/to/bibliography/file.bib"))

;; define markdown citation formats
(defvar markdown-cite-format)
(setq markdown-cite-format
      '(
        (?\C-m . "[@%l]")
        (?p . "[@%l]")
        (?t . "@%l")
        )
      )

;; wrap reftex-citation with local variables for markdown format
(defun markdown-reftex-citation ()
  (interactive)
  (let ((reftex-cite-format markdown-cite-format)
        (reftex-cite-key-separator "; @"))
    (reftex-citation)))

;; bind modified reftex-citation to C-c[, without enabling reftex-mode
(define-key markdown-mode-map "\C-c[" 'markdown-reftex-citation)

;; Additional configuration to allow invoking this with the traditional SPC menu in Spacemacs
(spacemacs/set-leader-keys-for-major-mode 'markdown-mode "ir" 'markdown-reftex-citation)
```

You can add this function to your Emacs `init.el` after which you can invoke it by typing `Ctrl-C-[` or in spacemacs by typing `SPC-m-i-r` when in Markdown mode. Note that the first invocation of this function can glitch where it will insert a `LaTeX` style citation but it works fine when you invoke it a second time.
