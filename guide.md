---
title: "BonBon Writing Guide"
description: "YAML fields, page variables, Markdown, and publishing in Merlin’s sandbox."
author: "Codex"
urlPrev: "https://bucket.contiguous.me/bonbon-sandbox/start.md"
urlNext: "https://bucket.contiguous.me/bonbon-sandbox/notebook.md"
---
# {{page.title}}

Prepared by Codex for Merlin Mann

Save a `.md` file in the bonbon-sandbox folder on Jonesy, then reload its live address in BonBon. YAML at the top holds page information; expressions such as `{{page.title}}` insert that information into the text. Front matter is optional: an ordinary Markdown file works too.

## A Complete Starting Template

Copy this entire block into a new Markdown file. The author field identifies the author of the eventual page; replace it if appropriate.

```markdown
---
title: "My New Page"
description: "A short description."
author: "Merlin Mann"
topic: "Things worth poking at"
urlPrev: "https://bucket.contiguous.me/bonbon-sandbox/start.md"
---

# {{page.title}}

{{page.description}}

By {{page.author}}

## {{page.topic}}

Write the page here.

[Back to the sandbox](start.md)
```

The file’s name determines its address. A file named `new-page.md` appears at:

```text
https://bucket.contiguous.me/bonbon-sandbox/new-page.md
```

Changing `title` does not rename the file or change its URL. New files appear automatically in [All pages](pages.md), linked near the top of the start page. The start page’s other links are hand-edited.

## All Eight Documented Page Fields

Every field is optional. Names are case-sensitive: `pubDate` and `urlNext` use capitals exactly as shown.

| Field | Purpose | Visible behavior |
| --- | --- | --- |
| `title` | Names the page. | Sets the BonBon tab title. A heading in the page still needs ordinary Markdown or `{{page.title}}`. |
| `description` | Holds a short description. | Does not insert a description into the body automatically. Use `{{page.description}}` where it belongs. |
| `author` | Names the author. | Does not add a byline automatically. Use `{{page.author}}`. |
| `pubDate` | Holds the publication date and time. | `{{page.pubDate}}` displays a valid date in the reader’s local time and locale. |
| `link` | Points to an original or related page. | Makes an expanded `{{page.title}}` a link to this address. It does not redirect the entire page. |
| `urlPrev` | Gives the previous page’s address. | Supplies the previous-page arrow. |
| `urlNext` | Gives the next page’s address. | Supplies the next-page arrow. |
| `rightMarginImage` | Gives an image’s address. | Places an image at the upper right, with text wrapping around it. |

This is a reference block containing every field. The date is an example, not a required default. The title-link and image fields are commented out so pasting the block does not activate nonexistent URLs. Replace those URLs with real addresses and remove the leading `#` only when those features are wanted.

```yaml
---
title: "My Page"
description: "A short description of this page."
author: "Merlin Mann"
pubDate: "Mon, 23 Nov 2026 12:00:00 GMT"
# link: "https://example.com/original-page"
urlPrev: "https://bucket.contiguous.me/bonbon-sandbox/start.md"
urlNext: "https://bucket.contiguous.me/bonbon-sandbox/reading-room.md"
# rightMarginImage: "https://example.com/image.png"
---
```

The eight fields are the documented page-level vocabulary, not a limit on the number of fields. A custom field stores data for a page variable; inventing a field such as `font` or `backgroundColor` does not create a corresponding BonBon feature.

## Page Variables

The basic form is `{{page.name}}`. BonBon replaces it with the corresponding front-matter value after converting the Markdown. Spaces inside the braces are allowed: `{{ page.title }}` also works.

```yaml
topic: "Things worth noticing"
subtitle: "An experiment"
word_count: 600
featured: true
```

```markdown
## {{page.topic}}

{{page.subtitle}}

Word count: {{page.word_count}}

Featured: {{page.featured}}
```

Text, numbers, and booleans can be displayed. The example `word_count` is a manually supplied value; BonBon does not calculate a word count from that field’s name.

Use names such as `topic`, `word_count`, or `readingTime`. The current macro parser accepts ASCII letters, numbers, underscores, and dots. A key such as `reading-time` may be legal YAML, but `{{page.reading-time}}` is not recognized by this parser. Prefer underscores for multiword names. Names are case-sensitive.

### Nested Values

Indent related values with spaces, then address the individual value with dots:

```yaml
source:
  title: "A source title"
  author: "A source author"
  url: "https://example.com/source"
```

```markdown
Source: {{page.source.title}}

Author: {{page.source.author}}
```

The whole `source` object cannot be displayed with `{{page.source}}`. The individual text values can. Similarly, a whole YAML list is not expanded into a list of Markdown items. Write presentation lists in the Markdown body.

### What Does Not Expand

- Variables inside inline code or fenced code blocks remain literal. That is why the examples in this guide can show their syntax.
- A missing variable remains visible as its original expression. An explicit YAML `null` or an object value also remains unexpanded.
- Variables in link destinations or image URLs are not substituted. The current implementation visits text nodes, not HTML attributes.
- Variable values are inserted as text. Markdown or HTML inside a value is not rendered a second time.
- This is not the Liquid template language. There are no documented loops, conditions, filters, calculations, or includes in page macros.

This does **not** build a working dynamic destination:

```markdown
[Read the source]({{page.source.url}})
```

Use a literal Markdown destination instead:

```markdown
[Read the source](https://example.com/source)
```

The special `title` plus `link` behavior is handled by BonBon itself. It does not mean variables work in arbitrary link destinations.

The custom-variable examples require both parts: the YAML definitions at the top and the matching expressions in the body. Copying only `{{page.subtitle}}` does not create a subtitle value. Typora displays BonBon expressions literally; use BonBon to see their expanded values.

## YAML Syntax That Matters Here

The opening `---` must be on the first line of the file. Do not place a heading, comment, or blank line above it. Close the front matter with another `---` line, then write Markdown below. BonBon also accepts `...` as the closing delimiter; the templates use `---` consistently.

Use `key: value`, with a space after the colon. Indent nested values with spaces rather than tabs. Quote titles, URLs, dates, and other text to avoid accidental YAML interpretation. A colon followed by a space, or a hash preceded by a space, is a good reason to quote a value.

```yaml
title: "Notes: a small experiment"
description: "The heading contains a # symbol."
quotation: 'He said "hello."'
word_count: 600
featured: true
status: "true"
# This is a YAML comment, not page text.
```

Here `featured` is a boolean; `status` is text. Avoid duplicate keys. To omit an optional feature, remove the line rather than leaving its value blank.

YAML supports multiline text:

```yaml
description: >-
  This folds several source lines
  into one text value.
note: |-
  First line.
  Second line.
```

`>-` folds ordinary line breaks into spaces; `|-` preserves them in the value. An expanded text value does not automatically become separate HTML paragraphs or forced visible line breaks. Write paragraphs in the Markdown body when their arrangement matters.

BonBon uses `js-yaml` to parse the block. Invalid YAML can leave the page without usable front-matter values; the parser removes a closed front-matter block even when parsing fails. An unclosed block is not recognized as front matter. If a title disappears or variables remain literal, check the opening line, closing delimiter, indentation, quotes, and key spelling.

## Dates

Use a quoted RSS-style date with an explicit time zone:

```yaml
pubDate: "Mon, 23 Nov 2026 12:00:00 GMT"
```

```markdown
Published {{page.pubDate}}.
```

That example means noon UTC, not noon in California. BonBon formats a valid `pubDate` in the reader’s own time and locale. It is not automatically the save time or publication time; the value is whatever the author supplies. An unparseable date falls back to its text value.

For an exact label that should look the same to everyone, use a custom string instead:

```yaml
date_label: "November 23, 2026"
```

```markdown
{{page.date_label}}
```

## Links and Page Arrows

Relative links work between files in this sandbox:

```markdown
[No One Man](bonk.md)
[Foo](Lorem.md)
[Back to the sandbox](start.md)
```

Match the filename’s capitalization exactly. `Lorem.md` and `lorem.md` are different web addresses even if the local Mac filesystem treats their names alike. Simple filenames such as `new-page.md` make links easier to maintain.

Use complete live URLs for `urlPrev` and `urlNext`, as the templates do. The arrows describe a reading sequence; they need not match every branch of a story. To make a title itself a link, supply both `title` and `link`, then use `{{page.title}}` in the body. A manually written heading such as `# My Page` does not acquire that behavior.

BonBon can derive arrows from a site table of contents when neither neighbor field is supplied. This sandbox uses explicit arrows and ordinary links; it has no site table of contents or calendar.

## Images and Appearance

An ordinary Markdown image on a paragraph of its own is centered by BonBon, with its alt text used as a caption:

```markdown
![A description of the picture](https://example.com/picture.png)
```

An image followed by text in the same paragraph floats to the right of that paragraph:

```markdown
![A description](https://example.com/picture.png) Text that accompanies the picture.
```

`rightMarginImage` places a picture at the page’s upper right. These are alternative ways to place an image; using both can display the image twice. Use a real, publicly readable image URL. The sandbox publisher currently uploads Markdown only, so saving a PNG in this folder does not publish it.

The reader chooses appearance with BonBon’s **A** button. Page YAML cannot supply CSS, fonts, or executable JavaScript. Ordinary HTML in the Markdown is stripped. Custom reader styles belong to the BonBon application, not the page’s front matter.

## Saving and Publishing in This Sandbox

The working folder is `~/Documents/Repositories/hub/projects/bonbon-sandbox` on Jonesy. It belongs to the Hub checkout; it is not a separate local clone of the public sandbox repository.

1. Create or edit a top-level file ending in `.md` in that folder.
2. Save the file. The publisher waits for two matching reads, two seconds apart.
3. Reload the page in BonBon. The live page is served from `https://bucket.contiguous.me/bonbon-sandbox/`.

The publisher uploads the file to Tiborg, checks the public contents, then commits the same contents to `merlinmann/bonbon-sandbox` on GitHub. The completed save tests appeared live in about five to six seconds and reached GitHub in about eight to nine seconds. Those are measured examples, not a guaranteed deadline; connectivity and service responses affect timing.

The live route disables both HTTP caching and nginx’s file cache. The old `raw.githubusercontent.com` addresses still work, but GitHub can cache them. Use the live address for immediate reloads.

Jonesy must be awake and connected to publish new saves. Previously published pages remain online when it sleeps. The watcher retries after errors; an already open BonBon page still needs a reload because this sandbox does not provide a live-update websocket.

### What Is Published

Only top-level, nonhidden `.md` files are published. Supporting files, subfolders, symlinks, images, and other file types are excluded. The publisher refuses Markdown files over 2 MB. All published page text is public on both the live site and GitHub; YAML metadata is public too.

Removing a local file does not remove its published copy. Renaming creates a new page address and leaves the old published address available. Codex can handle removals and link updates when requested.

The local folder is the normal editing location. GitHub edits do not download into it through this publisher. If the same file changes on GitHub and locally, the publisher can stop that publication to preserve the competing edit; Codex handles reconciliation.

`pages.md` is the generated page index. The publisher updates it when files are added or removed; edit the individual pages rather than this index. Creating a new file does not replace the content of `start.md` or another page already open in BonBon. Follow its link from All pages, or open its own address.

## When Something Looks Wrong

| Symptom | Likely check |
| --- | --- |
| The old text remains visible. | Reload BonBon and confirm the address begins with `https://bucket.contiguous.me/bonbon-sandbox/`, rather than the old GitHub raw address. |
| The tab title changed, but the heading did not. | A literal heading stays literal. Use `# {{page.title}}` to follow the YAML title. |
| A variable remains visible. | Check the exact key, its spelling and value type, and whether the expression is inside code. |
| A link fails. | Match the filename and its capitalization, including the `.md` suffix. |
| A new page is not listed among the start page’s examples. | Open All pages near the top of the start page. That list updates automatically; the example links are hand-edited. |
| A picture does not appear. | Check its public URL. Local image files are outside the Markdown publisher’s scope. |
| A saved edit never appears. | Jonesy may be asleep, connectivity may be unavailable, or publication may have stopped on an error or competing GitHub edit. Codex can inspect the publisher and complete the repair. |

No terminal work is needed for ordinary writing. The installation, logs, recovery paths, and server configuration are documented in the local `Supporting files/Publishing.md` for maintenance.

## Sources and Scope

Checked November 23, 2026. This guide covers the sandbox’s installed publisher and BonBon’s documented page fields. Parser details were checked against upstream revision `6a2ae03386bdc9999a8d3d5a01d7c139ee2780b5`.

- [BonBon’s usage and authoring guide](https://github.com/scripting/bonBon/blob/main/docs/bonbonSkillFromClaude.md)
- [The front-matter parser](https://github.com/scripting/bonBon/blob/6a2ae03386bdc9999a8d3d5a01d7c139ee2780b5/code/frontMatter.js)
- [The page-variable implementation](https://github.com/scripting/bonBon/blob/6a2ae03386bdc9999a8d3d5a01d7c139ee2780b5/code/index.js)
- [The public sandbox repository](https://github.com/merlinmann/bonbon-sandbox)

Site-level settings such as search, mirrors, and live-update sockets belong in a separate `.well-known/bonbon.json` file, not in page YAML. The current sandbox does not implement those services. They are not additional page-front-matter options.

[Back to the sandbox](start.md)
