---
title: "Fix Method Not Allowed Error with OwnCloud"
date: 2020-05-27T16:50:49+08:00
draft: true
---

As part of my shift to plaintext based tools, I was looking for a good Markdown editor on iOS as well as an App that I could use to maintain a personal journal. For Markdown editing, I settled on 1Writer and for my journal I decided to use Notebooks. However, with both tools when I tried to connect them to my self-hosted OwnCloud instance I would get a "Method Not Allowed" error.

Turns out it's not sufficient to point the apps to the "/" endpoint on OwnCloud. You have to specify the WebDAV endpoint which in OwnCloud is typically "example.org/owncloud/remote.php/webdav"
