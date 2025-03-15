---
id: "5"
author: "hlassiege"
title: "Announcing Bloggrify 2.1: More Shortcodes and Analytics Integration"
description: "Minor release with new shortcodes and Umami analytics integration. "
date: "2025-01-24"
tags:
  - release

---

We're excited to announce the release of Bloggrify 2.1.0! This update improve existing features to enhance your blogging experience: shortcodes and built-in analytics integration.

## What's New in 2.1.0

### 1. Shortcodes: Simplify Complex Content

Shortcodes provide an elegant way to insert complex elements into your Markdown content without writing lengthy HTML or Vue components. 
Here are the new shortcodes available in Bloggrify 2.1.0:

- youtube: Embed YouTube videos with a simple shortcode
::mdd

#preview    
:youtube{videoId="su2gNQJkteg"}


#markup
```markdown
:youtube{videoId="su2gNQJkteg"}

```
::
- X: Embed tweets directly in your content
  ::mdd

#preview    
:X{tweetId="1750435525071159309"}


#markup
```markdown
:X{tweetId="1750435525071159309"}

```
::
- instagram: Display Instagram posts with ease
::mdd

#preview    
:Instagram{postId="CxOWiQNP2MO"}


#markup
```markdown
:Instagram{postId="CxOWiQNP2MO"}

```
::
- vimeo: Add Vimeo videos to your blog posts

::mdd

#preview    
:Vimeo{videoId="55073825"}


#markup
```markdown
:Vimeo{videoId="55073825"}

```
::

### 2. Analytics Integration

Umami has been added as a new analytics provider in Bloggrify 2.1.0. 

You can now use it alongside Google Analytics, Blogtally pulse, Fathom, pirsch, and Plausible to track your blog's performance.


```ts
// app.config.ts
analytics: {
    providers: [ {
        provider: 'umami',
        code: 'YOUR_TRACKING_CODE'
    }]
 }
```

## Upgrading to 2.1.0

Upgrading to Bloggrify 2.1.0 is straightforward:

```bash
npm install bloggrify@latest
```

For existing projects, no configuration changes are required unless you want to use the new features.

## Contribute
Bloggrify is open source. [Help us on GitHub](https://github.com/bloggrify/bloggrify):

* Report issues
* Submit PRs
* Join discussions
