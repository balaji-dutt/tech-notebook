---
title: "Show custom Python venv name in Windows Command Prompt"
date: 2020-07-01T17:44:11+08:00
draft: false
---

As part of my workflow for my Zettelkasten notes, I've been creating a set of Python scripts that change the wikilink-style links that most Zettelkasten software use into something better understood by standard Markdown renderers. I used Python venvs for these scripts and discovered one annoyance - if you use Windows Command Prompt (`cmd.exe`) as your default terminal in PyCharm or elsewhere, when you activate the venv, the environment name that appears in the prompt will the name of the venv folder and not of your git repo where you are developing the scripts. This is even more annoying in that if you use PowerShell, then the activation script correctly reads this value from `pyenv.cfg` but the same does not apply for the `activate.bat` script.

To fix this, open up `activate.bat` and change the text in brackets on Line 19, i.e. this line:

```shell
set PROMPT=(venv) %PROMPT%
```

Once you close & exit any running terminal windows, the new name should get picked up the next time you activate the venv.

Tested with: Python 3.8.3 on Windows 10.
