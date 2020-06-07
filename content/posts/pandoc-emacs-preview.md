---
title: "Preview Markdown files in Emacs with pandoc plus Live preview!"
date: 2020-05-28T17:51:20+08:00
draft: false
---
The markdown-mode in Emacs does ship with a basic preview mode where it will show the Markdown content as HTML, but there are no CSS styles applied (naturally) and this makes it difficult to get a sense of how the page would render on GitHub. spacemacs ships with a extra function that renders the Markdown content in an Emacs window with GitHub formatting, but again this is without CSS styling. So let's use pandoc instead!


#### Using pandoc to render a fully styled preview
* Download the pandoc HTML [GitHub template](https://github.com/tajmone/pandoc-goodies/blob/master/templates/html5/github/GitHub.html5) as a "Raw" file
* Place the file in a "templates" folder in the Pandoc user-directory. In Windows the user-directory is ```%APPDATA%\pandoc```
* In spacemacs, configure the Markdown layer as follows:
```elisp
(markdown :variables
               markdown-command "pandoc -t html5 -f markdown_mmd --self-contained --mathjax --quiet --highlight-style=zenburn --template github.html5"
               markdown-live-preview-engine 'pandoc)
```
   You can replace ```markdown_mmd``` with ```gfm``` if you want to fully replicate GitHub rendering.


#### Bonus - Live Preview any Markdown file with Emacs
This is a pretty nifty feature that uses stock Emacs capabilities, but I'll mention some extra spacemacs goodies at the end.
* First, make sure the emacs httpd package is available and is configured to run on port 7070
```elisp
(use-package simple-httpd
:defer t
:config
(setq httpd-port 7070)
;; (setq httpd-host (system-name))
)
```
* Next configure a function to render Markdown content using the GitHub CSS format.
```elisp
(defun my-markdown-filter (buffer)
"Configure a default layout for rendering markdown files"
(princ
(with-temp-buffer
(let ((tmp (buffer-name)))
(set-buffer buffer)
(set-buffer (markdown tmp))
(format "<!DOCTYPE html><html><title>Markdown preview</title><link rel=\"stylesheet\" href = \"https://cdnjs.cloudflare.com/ajax/libs/github-markdown-css/3.0.1/github-markdown.min.css\"/>
<body><article class=\"markdown-body\" style=\"box-sizing: border-box;min-width: 200px;max-width: 980px;margin: 0 auto;padding: 45px;\">%s</article></body></html>" (buffer-string))))
(current-buffer)))

```
* Next configure a function to start the httpd server and render the Markdown content
```elisp
(defun my-markdown-live-preview ()
"Live preview markdown."
(interactive)
(unless (process-status "httpd")
(httpd-start))
(impatient-mode)
(imp-set-user-filter 'my-markdown-filter)
)
```
* Finally, add a hook to markdown-mode to launch the httpd server whenever a markdown file is opened.
```elisp
(with-eval-after-load 'markdown-mode
(add-hook 'markdown-mode-hook 'my-markdown-live-preview)
)
```
   - You can now go <http://localhost:7070>" in your browser and you will a see list of markdown files open in Emacs. Click on any file and get a live-updating preview in GitHub styling!
   - When you quit Emacs after opening a Markdown file, you will get a prompt that a network process is open. It's safe to press "y" on this prompt.
* Bonus Bonus: In spacemacs, you can add a little tweak to make the live-preview get a nice icon in the modeline. Just add the following to your spacemacs init.el:
   ```elisp
   (spacemacs|diminish impatient-mode " Ⓘ" " I")
   ```
