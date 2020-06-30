---
title: "Fix Sultan Command Execution error in Windows"
date: 2020-07-01T18:26:21+08:00
draft: true
---

I came across [Sultan](https://sultan.readthedocs.io/en/latest/) many months ago and had filed it away for future reference. Cue the work I've been doing on automating my Zettelkasten workflow in Python and in order to simplify calling out to an external program (no points for guessing that the program I'm calling is pandoc) I decided to use Sultan.

In general, I found using Sultan a pretty smooth experience with the exception of one annoying glitch. When you use Sultan to call an external program it appends a `;` character to the end of the command. This works fine in Bash but causes weird behaviour in Windows. It turns out to be [a known issue](https://github.com/aeroxis/sultan/issues/64) but so far the package maintainers have not even acknowledged that the issue exists. However, it can be fixed manually.

Open the file `api.py` in the sultan folder under `site-packages` and go to Line 262. You should see the following:

```python
output = output.strip() + ";"
```

Just remove the `+ ";"` part so it looks like this instead:

```python
output = output.strip()
```

That should fix the issue of the unwanted `;` character.

Tested with: Sultan 0.9.1 and Python 3.8.3 on Windows 10.
