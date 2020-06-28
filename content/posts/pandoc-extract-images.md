---
title: "Extract and rewrite image paths in Markdown files using Pandoc"
date: 2020-06-28T15:08:35+08:00
draft: false
---

More document manipulation tricks using pandoc! This time I'm using pandoc to change image paths in Markdown documents - very useful for example, when publishing Zettelkasten notes written in Markdown files to a blog.

```shell
pandoc X:\path\to\source.md -f markdown ---standalone  --resource-path= --resource-path=.;"X:\\path\\to\\parent\\folder\\\";"X:\\path\\to\\another\\folder\\" --extract-media=/img  t markdown_mmd+yaml_metadata_block -o X:\path\to\content\posts\target.md"
```

This will make pandoc search for linked images and place them in a folder named `img` using the file SHA as the filename. It will also change the link in the output markdown file to point to the new location - neat! Few things to watch out for:

* The path specified in the `--resource-path` parameter must be to the parent folder of the folder where the images are stored. In other words, if the Markdown file has a image link `![](./images/pandoc.jpg)` and the path to the `images` folder is `X:\parent\folder\images`, then the path specified in the `--resource-path` parameter would be `X:\parent\folder`. pandoc will automatically look for a folder name `images` in that path.
* To include spaces in a path in Windows, you can use double quotes as long as you escape all back-slashes.
* The parameter can only be specified once. So if you pass the parameter as `--resource-path="X:\\path\\to\\parent\\folder\\" --resource-path="X:\\path\\to\\another\\folder\\"` then pandoc will look for images only in `X:\\path\\to\\another\\folder\\"`. Hence to specify multiple folders append paths to the same `--resource-path` parameter.
* The path for `--extract-media` is tricky. It should be the path as per your website structure - so if images are served on the website from a `/images` path then this parameter should also be set to `/images`. However the tricky part is that pandoc then expects a folder called `/images` to be present in the local filesystem as well. This means that in Linux, pandoc will create a folder under the "/" partition and in Windows, pandoc will create a folder directly under "X:" where X: happens to be the drive within which pandoc is running when invoked. This can result in images not appearing the right folder location for your actual website structure in your local machine - the only way to overcome this is to create a symlink in the "/" or "X:" partition pointing to the right location.
* By default pandoc looks in the current directory it is running in for images when no `--resource-path` is supplied but `--extract-media` is. However, that folder is _not searched_ if you specify a folder using the `--resource-path` option, so you should manually include it by adding ".;" to the `--resoruce-path` parameter. Oh and remember to use the right path separator - ";" in Windows and ":" for Linux, macOS.
