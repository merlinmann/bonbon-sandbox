---
title: "Under the Hood"
author: "Codex"
urlPrev: "https://bucket.contiguous.me/bonbon-sandbox/notebook.md"
urlNext: "https://bucket.contiguous.me/bonbon-sandbox/start.md"
---
# {{page.title}}

Prepared by Codex for Merlin Mann

## What Is Here

Markdown pages hosted on Tiborg, with a public GitHub copy. No build is needed. BonBon supplies the typography. Published pages remain readable when Jonesy is asleep.

Links between the pages use relative paths. Each page’s front matter supplies its tab title and explicit previous/next addresses. The heading uses BonBon’s page-title macro.

## What to Change

The [notebook](notebook.md) is a convenient starting point. Changing `title` changes both the tab title and the heading in BonBon. Ordinary Markdown links can lead to new pages.

GitHub’s own preview does not expand BonBon’s macros. Use BonBon to see the rendered result. A small save publisher on Jonesy checks this folder every two seconds and publishes a change after two matching reads. The live server disables caching. Reloading in BonBon retrieves the current published page.

## What This Setup Does Not Include

There is no site search, live-update socket, or calendar. Saving publishes automatically; a page already open in BonBon still needs a reload. Explicit page arrows and ordinary links provide navigation.

## References

- [BonBon](https://github.com/scripting/bonBon)
- [Dave Winer’s first-page instructions](https://github.com/scripting/bonBon/blob/main/docs/helloworld.md)
- [BonBon’s authoring and usage guide](https://github.com/scripting/bonBon/blob/main/docs/bonbonSkillFromClaude.md)

[Back to the sandbox](start.md)
