---
pageid: "15"
author: "hlassiege"
title: "Announcing Bloggrify 3.2: redesigned minimalist theme, AI crawler controls, and faster pages"
description: "The default theme gets a full redesign with table of contents, newsletter and share buttons, alongside new SEO controls for AI crawlers, WebP images and on-demand search."
date: "2026-07-18"
table_of_contents: true
tags:
    - release
---

Bloggrify 3.2.0 is out. This release redesigns the default minimalist theme, adds a set of SEO controls aimed at AI crawlers, makes pages lighter for the average reader, and fixes a number of correctness issues around drafts, authors and error pages.

## A redesigned minimalist theme

The minimalist theme, the one Bloggrify ships with, has been rebuilt after the Nuxt UI blog template. It is no longer just a list of posts:

- a **table of contents** on posts, which you can hide per post with the `notoc` frontmatter
- a **newsletter signup form**
- **social share buttons** on articles

The redesign leans on Nuxt UI components throughout, so it stays consistent with the rest of the stack and inherits its dark mode.

## Controls for AI crawlers and llms.txt

A new `seo` config key lets you decide how AI tools interact with your blog, without touching your search engine indexing. Blocking the major AI crawlers is one line, and publishing an `llms.txt` index is another. Both default to leaving your blog exactly as it is today.

We wrote a full post on what these switches do, what they actually protect, and whether they are worth turning on: [Should you block AI crawlers on your blog?](/2026/ai-crawlers-and-llms-txt).

## Lighter pages by default

Two changes move cost off the readers who were paying for features they did not use:

- **Article images are served as WebP**, which are smaller than the equivalent JPEG or PNG.
- **The search index loads on first search intent only.** Until now every page quietly downloaded the search engine and content database, even for readers who never searched. It now loads when a reader hovers the search button or opens it with ⌘K.

The search change turned out to be more subtle than flipping a flag, so it also has its own write-up: [The search box that downloaded itself on every page](/2026/loading-search-only-when-readers-need-it).

## A new comment provider: Hakanai Connect

Bloggrify gains **Hakanai Connect** as an alternative to Hyvor Talk for comments. It is now the provider running on the official Bloggrify blog. Hyvor remains supported, so switching is a config choice, not a migration you are forced into.

## More analytics providers

Two providers join the existing options, and analytics config is now fully optional:

- **OpenPanel**
- **Hakanai Pulse**

## Authors and error pages

Author handling gets more predictable:

- an **opt-in authors directory page**
- the post author is now part of the **SEO metadata**
- Bloggrify **warns in dev** when a post references an author you never defined
- posts without an explicit author show up on the default author page instead of disappearing

Themes can now ship **their own error pages**, with a per-theme override and a sensible fallback when a theme does not provide one.

## Correctness fixes worth calling out

- drafts and unlisted pages **no longer leak into the RSS feed**
- unknown URLs and pages with no matching layout return a proper **404 instead of a 500**
- previous/next article navigation is now ordered **by date** rather than by file path
- `draft` and `listed` frontmatter behave as the documentation describes

## Under the hood

Bloggrify 3.2 moves to **Nuxt 4.4.8** and upgrades the Nuxt SEO modules (sitemap v8, robots v6, schema-org v6). Lucide icons are now bundled locally with `@iconify-json/lucide`, and `nuxt-og-image` was upgraded to v6 to restore OG image generation.

## Contribute

Bloggrify is open source. [Help us on GitHub](https://github.com/bloggrify/bloggrify):

- Report issues
- Submit PRs
- Join discussions
