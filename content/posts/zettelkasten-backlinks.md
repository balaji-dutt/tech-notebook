---
title: "Automating Backlinks creation for Zettelkasten notes"
date: 2020-05-27T15:41:29+08:00
draft: false
---

Somewhere in parallel with my Emacs explorations, I came across the concept of [Zettelkasten](https://zettelkasten.de/posts/overview/) and I was instantly hooked. Much like GTD, Zettelkasten is a technique that's "Easy to understand. Hard to apply" but my early experience with it has been very promising.

One of the ongoing debates in the Zettelkasten community is around the question of backlinks - basically how should you maintain the inter-relationships between your notes that are the heart of a Zettelkasten system? There's plenty of arguments for and against out there so I'm not going to debate that here. For people who are looking to have a tool manage this process, there are a couple of options:

### Using Andy Matuschak's Note Link Janitor
This is the easier option since it is available in the npm repositories but there are some caveats to using it with most Zettelkasten tools.
* The tool only works with Markdown files.
* To be able to use this tool from the command-line, make sure to add ```%USERPROFILE%\node_modules\.bin``` to your %PATH% variable.
* The Zettel must start with a Level 1 Heading, i.e. it should start with ```# Title```.
* The Zettel title must be the same as the filename without the trailing ".md" extension
* The Link to a Zettel must be in the format ```[[Full Title as per Zettel Note]]```

The specific title requirements are a problem since a lot of Zettel tools do not use this particular format. Thankfully the folks in the Zettelkasten forums have forked Andy Matuschak's tool to work well with Zettelkasten tool but again there are some steps to be followed to get the fork working

### Using the Zettelkasten Fork of Note Link Janitor
* Remove the Note Link Janitor package from Andy Matuschak if you have it installed
* Clone the fork from [this GitHub repo](https://github.com/PiotrSss/note-link-janitor)
* Run ```yarn build && yarn install```
    - On Windows, make sure to run these commands in **Bash** as the build script looks for a "chmod" command.
* Create a Batch file with the command ```node /path/to/note-link-janitor/dist/index.js``` and put it somewhere in your $PATH.
    - On Windows you can create a Batch file which is ``` node  "X:\path\to\note-link-janitor\dist\index.js" %*``` and place the batch file in a folder that's in your %PATH%
* You should now be able to run ```note-link-janitor.bat X:\Path\to\Zettlels\```
* The forked version supports the following (which is the default for how most Zettelkasten tools work):
    - Notes that start without a title
    - Links that use the syntax ```[[YYYYMMDDHHMM(SS)]]``` without the rest of the title

#### Installing Node in Windows
The tool (both original and fork) are node.js packages and hence require Node to be installed. On Windows, there is a default node.js installer available, but since that's the easy option I decided to use [nvm](https://github.com/coreybutler/nvm-windows) instead. nvm does have the benefit of allowing multiple node versions to co-exist on the same machine but I don't think it's quite as capable as a Python venv.
