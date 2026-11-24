---
title: "Under the Hood"
author: "Codex"
urlPrev: "https://raw.githubusercontent.com/merlinmann/bonbon-sandbox/main/notebook.md"
urlNext: "https://raw.githubusercontent.com/merlinmann/bonbon-sandbox/main/start.md"
---
# {{page.title}}

Prepared by Codex for Merlin Mann

## What Is Here

Seven Markdown pages, hosted as raw files in a public GitHub repository. No build, server, account with another host, or running Mac is needed to read them. BonBon supplies the typography.

Links between the pages use relative paths. Each page’s front matter supplies its tab title and explicit previous/next addresses. The heading uses BonBon’s page-title macro.

## What to Change

The [notebook](notebook.md) is a convenient starting point. Changing `title` changes both the tab title and the heading in BonBon. Ordinary Markdown links can lead to new pages.

GitHub’s own preview does not expand BonBon’s macros. Use BonBon to see the rendered result. GitHub raw URLs here track `main`, so committed edits appear at the same addresses after the cache refreshes.

## What This Setup Does Not Include

There is no site search, live-update socket, or calendar. BonBon looks for a site’s table of contents at the domain root; a folder on `raw.githubusercontent.com` cannot supply that root file. Explicit page arrows and ordinary links work without it.

## References

- [BonBon](https://github.com/scripting/bonBon)
- [Dave Winer’s first-page instructions](https://github.com/scripting/bonBon/blob/main/docs/helloworld.md)
- [BonBon’s authoring and usage guide](https://github.com/scripting/bonBon/blob/main/docs/bonbonSkillFromClaude.md)

[Back to the sandbox](start.md)
