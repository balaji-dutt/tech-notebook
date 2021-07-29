---
title: "Configure Confluence Live Search when using the Global theme"
date: 2020-07-05T15:40:58+08:00
draft: false
---

If you are using Confluence, you might have come across an annoying behaviour when using the type-ahead search that appears in the left navigation bar - the search bar will search across all available "Spaces" in the Confluence site which on a big shared site, can result in a lot of spurious hits. The [Confluence documentation](https://confluence.atlassian.com/conf59/switch-to-the-default-theme-792498652.html) states that this is by design when using the default theme.

Thankfully, there is a way to fix the search so that it only searches within a particular Confluence" "Space". This trick does require you to have at least Space Admin rights though. Navigate to Space tools > Look and Feel and then look for the tab "Sidebar,header and footer". If there is a `{livesearch}` macro listed in the sidebar, change it as follows:

```shell
{livesearch:spacekey=MY_CONFLUENCE_SPACE|placeholder=Any placeholder text}
```

Once you make this change, the type-ahead search will automatically be restricted to a single Confluence "space". This is also documented in the [Confluence documentation](https://confluence.atlassian.com/doc/livesearch-macro-163415902.html)

Tested on: Confluence Server 6.6.9
