---
pageid: "14"
author: "hlassiege"
title: "The Search Box That Downloaded Itself on Every Page"
description: "How Bloggrify's instant search worked, why it was quietly costing every reader a download they never used, and how we made it load only on intent."
date: "2026-07-17"
table_of_contents: true
tags:
    - performance
    - nuxt
---

Bloggrify ships an instant, fuzzy search: hit ⌘K, start typing, and results appear as you go. It feels free. It was not: the download that powers it was being paid by every reader, including the majority who never search. This post looks at why, and how we changed it so the cost falls only on the readers who actually use it.

## How search works when there is no server

Bloggrify blogs are static. There is no backend, no database sitting behind an API, nothing to send a query to. So when a reader searches, the searching has to happen in their browser.

That is a lovely property. It also has a consequence: if the browser is going to do the searching, the browser needs the thing to search. Nuxt Content builds that "thing" for you, and under the hood it is not small. To run queries client-side, it loads a WebAssembly build of SQLite and a dump of your content database. The browser boots a tiny database engine and asks it questions locally.

That is what makes the search feel instant once it is running. It is also a real download, and downloads are never free.

## The catch: everyone paid, whether they searched or not

Here is the code we started from, roughly:

```typescript
const { data: files } = useLazyAsyncData(
  'search',
  () => queryCollectionSearchSections('page'),
  { server: false }
)
```

Read that carefully. `server: false` says "do not run this during the static build, run it in the browser". Sensible: the search index is only useful client-side. But `useLazyAsyncData` fetches **immediately** on mount by default. And the search component lives in the navigation bar, which is on every single page.

So the chain was: every page mount, on every visit, quietly kicked off the fetch of the WASM engine and the database dump. For a reader who lands on one post, reads it, and leaves, that is a download of the entire search machinery to power a feature they never touched. Most readers never search. We were charging all of them for the few who do.

## Why you cannot just "load it later"

The obvious fix is to stop loading it eagerly. Nuxt gives you exactly that switch:

```typescript
const { data: files, status, execute: loadSearchIndex } = useLazyAsyncData(
  'search',
  () => queryCollectionSearchSections('page'),
  { server: false, immediate: false }
)
```

`immediate: false` means nothing is fetched until you call `execute()` yourself. Now the reader who never searches downloads nothing. Good.

But this is where the interesting part starts, because the naive version of "load it later" is worse than the problem it solves. If you wait until the reader clicks the search button to start the download, they open the box, see an empty field, and wait. The engine and the dump have to arrive before they can get a single result. You have taken a feature that felt instant and made it stutter for the exact people who wanted to use it.

The index needs to be there the moment they want it, and not a moment before they show they want it. The whole trick is reading that intent early enough.

## Reading intent, from two directions

The challenge is that there are two completely different ways to open search, and they give you different amounts of warning.

The mouse is generous. Nobody clicks a button without first moving the pointer to it, and hovering is a near-certain signal that a click is coming. That gap of a few hundred milliseconds between hover and click is exactly enough to start the download early. So we prefetch on hover and on keyboard focus:

```html
<UContentSearchButton
  @pointerenter="ensureSearchIndex"
  @focus="ensureSearchIndex"
/>
```

By the time the click lands, the index is usually already on its way, often already there.

The keyboard shortcut is the opposite. ⌘K gives no warning at all: intent and action are the same instant. There is no hover to catch. So we cover it from the other side, by watching the state that actually represents "search is open", whichever path opened it:

```typescript
const { open } = useContentSearch()

watch(open, (isOpen) => {
  if (isOpen) {
    ensureSearchIndex()
  }
})
```

`open` is a shared piece of state toggled by the button and by the shortcut alike. Watching it means we do not have to enumerate every possible trigger. Anything that opens search, present or future, flows through here.

Finally, all of these paths funnel through one small guard so the index is fetched once and not on every hover:

```typescript
function ensureSearchIndex() {
  if (status.value === 'idle' || status.value === 'error') {
    loadSearchIndex()
  }
}
```

It loads when nothing has happened yet (`idle`), and it retries if a previous attempt failed (`error`), but it stays out of the way while a fetch is in flight or already done. The reader can wave the pointer over the button ten times and only the first one does anything.

## A small tuning while we were here

Once search actually loads on demand, it is worth making sure the results are good. Fuzzy matching is handled by Fuse.js, and its defaults are tuned for short strings, not blog content. Two options made a real difference:

```typescript
:fuse="{ resultLimit: 42, fuseOptions: { threshold: 0.3, ignoreLocation: true } }"
```

`ignoreLocation` tells Fuse to stop caring where in the text a match occurs. By default it favours matches near the start of a field, which makes sense for names and titles but punishes a term that appears in the middle of a paragraph. `threshold: 0.3` tightens how fuzzy a match is allowed to be, so a search returns things that are genuinely relevant rather than everything vaguely alike.

## What changed, concretely

Nothing about the experience got slower, and for most visitors a good chunk of every page load simply disappeared. A reader who never searches now downloads none of the search machinery. A reader who does gets it warmed up before they have finished reaching for it, whether by mouse or by keyboard.

This pattern is not specific to search. Eager data fetching is the comfortable default, and it is very often the wrong one. `useAsyncData` will happily load everything the moment a component mounts, and that convenience quietly adds up when the component is everywhere and the data is heavy. Flipping to `immediate: false` and spending a little effort on reading intent is not much code. Here it moved a cost off every reader who was never going to use it.
