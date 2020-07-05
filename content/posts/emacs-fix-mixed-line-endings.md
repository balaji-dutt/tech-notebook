---
title: "Fix Mixed Line Endings in Emacs"
date: 2020-07-05T16:14:39+08:00
draft: false
---

I have been dealing with a strange heisen-bug with my Zettelkasten notes where occasionally a note will wind up with the wrong (or mixed) line endings after it has been updated by a script that inserts backlinks within the notes. I say heisen-bug since I can't reproduce it and I'm not 100% confident that it is the script that is triggering this problem.

The symptom of this problem is when the file is open in Emacs, you see ugly "^M" characters at the end of each-line. The easiest way to fix this is by running the following command:

```
M-x replace-string RET C-q C-m RET RET
```

Breaking it down, we are calling the `replace-string` function in Emacs and when prompted to enter the string to search for type `C-q C-m`. Typing `C-q` allows us to enter the literal `^M` character. Finally, we hit `RET` when prompted for the replacement text and that should result in all the wrong line-endings being removed.

Tested with: Emacs 26.3 on Windows 10.
