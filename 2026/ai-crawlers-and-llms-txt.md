---
pageid: "13"
author: "hlassiege"
title: "Should you block AI crawlers on your blog? And does it even work?"
description: "The major AI crawlers honour robots.txt, so blocking them is one line in Bloggrify. Whether you should, and what it actually protects, is another question."
date: "2026-07-16"
table_of_contents: true
tags:
    - seo
    - ai
---


Should you block AI crawlers on your blog?

It is a real question for a lot of writers online right now. Should we let LLMs feed off our content? Is there a risk to our visibility if, tomorrow, readers get their answers straight from ChatGPT?

Some articles are indeed alarmed by the fairly broad drop in traffic across sites.

Or the other way round: by becoming invisible to the LLMs, are we not cutting ourselves off from a potential source of traffic?

For my part, I tend to think that we, as readers, are changing the way we browse and search, and that this drop in traffic is unavoidable. So the question is whether to block the crawlers entirely to slow the trend down, or instead to try to be part of this new ecosystem. You be the judge.

Say you pick one path or the other. How do you go about it? How do you block the crawlers, or encourage them to read your blog? And does it actually work?

Before we start, it helps to separate two things that often get mixed up:

1. **Do you want AI crawlers to use your content at all?** That is a `robots.txt` question
2. **Do you want to make your content easy for AI tools to read?** That is what `llms.txt` is for

They are independent. You can block crawlers and skip `llms.txt`, publish `llms.txt` and welcome crawlers, or do neither. What does not make sense is answering "no" to the first and "yes" to the second, and we will come back to that.

## Do you want AI to read your blog?

First thing first: the major AI crawlers do respect `robots.txt`.

OpenAI, Anthropic, Google, Perplexity and the rest publish the names of their crawlers, and they document that those crawlers honour `robots.txt` directives. This is not a folk belief, it is written policy. So if you decide you would rather not feed your writing into a training set or an answer engine, you can say so, and the well-behaved bots will listen.

In Bloggrify that decision is one line in your `app.config.ts`:

```typescript [app.config.ts]
export default defineAppConfig({
  seo: {
    ai: {
      allowCrawlers: false,
    },
  },
})
```

That adds a `Disallow: /` group to your generated `robots.txt` covering the AI crawlers of the major vendors. Your blog stays perfectly indexed in Google and Bing, because search crawlers are deliberately left alone: you refuse the AI training agents without breaking the features you actually want. If you would like the full details of what gets blocked and what does not, the [AI crawlers reference](https://bloggrify.com/reference/ai) spells it out.

Two things are worth being clear about.

**It is a request, not a wall.** `robots.txt` is voluntary by design. Most crawlers honour it, some have been caught ignoring it, and anything you publish on a public static blog can ultimately be copied by someone who does not care about the rules. If you need real enforcement, you block at the network level, for example with your host's "block AI scrapers" feature. That happens outside Bloggrify, at your CDN or hosting provider.

**Doing nothing is a valid answer too.** If you do not mind AI tools reading your posts, you change nothing. That is the default. Your content stays open, and the crawlers come and go as they always have. Not every blogger wants to fight this, and that is completely fine.

The point is simply that it is your content and your call. Bloggrify gives you the switch so the choice is yours to make, not one made for you by omission.

## Making your blog easy to read: llms.txt

If you land on the "sure, AI can read my blog" side, there is a second, gentler thing you can offer: `llms.txt`.

`llms.txt` was proposed by Jeremy Howard, co-founder of Answer.AI, in September 2024. The idea is small and reasonable. A web page is full of navigation, scripts and markup that an AI has to wade through to find the actual content. `llms.txt` is a single markdown file at the root of your site that hands over a clean, structured index instead: a title, a short description, and a link for each post. You can read the full specification at [llmstxt.org](https://llmstxt.org).

Turning it on is, again, one line:

```typescript [app.config.ts]
export default defineAppConfig({
  seo: {
    ai: {
      llms: true,
    },
  },
})
```

Bloggrify then generates a `/llms.txt` route at build time, right next to your `sitemap.xml` and `rss.xml`. Drafts and hidden posts are excluded, and when the option is off, which is the default, the file is not generated at all.

### Should you bother?

Here I want to be straight with you rather than sell it.

`llms.txt` is an invitation, not an instruction. It does not tell anyone what they may or may not do with your content, it only makes the content easier to consume. That is exactly why enabling it while also blocking AI crawlers is contradictory, and Bloggrify prints a warning at build time if you try to do both.

The format has picked up real momentum, and you will find it on plenty of sites, especially documentation. But its actual usefulness today is still speculative. No major AI provider has confirmed that its crawlers read `llms.txt`, and Google has publicly compared it to the old `keywords` meta tag, which is not a flattering comparison. The one case that genuinely works is a human pasting your URL into a chat: the tool fetches the page, and clean markdown beats a page full of navigation every time.

For a documentation site, where structure is everything, that is a clear win. For a blog, where each post is already a clean, focused page, the gain is smaller. It costs you one tiny file. So enable it if the idea appeals to you, but do not expect traffic to appear because of it. It is proposed, it is standardised, and everyone gets to do what they like with it. Bloggrify simply gives you the switch.

## You decide

None of this is Bloggrify taking a side in the AI debate. It is two independent switches with defaults that respect you:

- **Crawlers are allowed by default.** Flip `allowCrawlers` to `false` the day you decide otherwise, and the major vendors will honour it.
- **`llms.txt` is off by default.** Flip `llms` to `true` if you want to publish a clean index, knowing it is an offer, not a guarantee.

If you want to go deeper, both are documented in the [AI crawlers reference](https://bloggrify.com/reference/ai), and indexing in search engines, which is a separate topic again, lives in the [robots.txt reference](https://bloggrify.com/reference/robots).

Bloggrify is open source. You can find the project [on GitHub](https://github.com/bloggrify/bloggrify).
