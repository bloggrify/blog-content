---
pageid: "8"
author: "hlassiege"
title: "Analytics in Bloggrify: Measuring Your Audience Without Tracking Them"
description: "How Bloggrify handles analytics: why cookieless providers matter, why Google Analytics is allowed but not recommended, and how to get accurate numbers past ad blockers."
cover: "/blog/analytics.jpg"
date: "2024-02-11"
tags:
  - analytics
---

Most blogs want the same two things from analytics: how many people came, and what they read. That is a modest question. The tools people reach for to answer it, led by Google Analytics, tend to collect far more than that, and they hand you a cookie banner and a privacy policy as the price of entry. 

Bloggrify encourages you to use privacy-focused analytics providers that take a narrower path on purpose, and this post explains why, and where the sharp edges are.

## Why cookieless analytics

The reason a site needs a cookie banner is not analytics as such, it is the cookies and the personal data. Under the GDPR and the ePrivacy directive, storing a cookie on a reader's device for tracking, or collecting data that identifies them, requires consent. That is what the banner is: a consent prompt the law obliges you to show.

Cookieless analytics sidestep the whole thing. Providers like Hakanai, Pirsch, Plausible, Umami, Fathom and OpenPanel can count visits without storing anything on the reader's device and without building a profile of who they are. They measure page views, referrers and rough geography from data the browser already sends, then aggregate it. No cookie, no personal data, and in the common case no banner to show. The tracking script is also a fraction of the weight of a typical tag manager, so the page stays fast.

None of that is automatic, though. It describes these tools in their default, privacy-preserving mode, not a guarantee they enforce. Most of them also offer opt-in features that set identifiers, follow individuals across sessions or enrich visitor data, and **the moment you turn those on you are collecting personal data again and back to needing consent**. Whichever provider you pick, check its settings and keep it in the cookieless mode this whole approach depends on.

Bloggrify integrates with these providers directly. You pick one, drop your site code into the config, and the script is added to every page.

## Google Analytics is allowed, not recommended

Bloggrify does not forbid Google Analytics. If you already run it everywhere and want it here too, the `google` provider is there. But it is a deliberate outlier in the list, and worth being honest about.

Google Analytics sets cookies and sends visitor data to Google, which puts it squarely in banner-and-consent territory. It has also been on shaky legal ground in Europe: several national regulators, in Austria, France and Italy among others, have ruled that sending European visitors' data to Google Analytics breached the GDPR. The legal picture keeps shifting, but the direction of travel is clear enough that building a new blog on it in 2024 is a strange choice.

I can't remove it because it's widely used, but I don't recommend it.

## The ad blocker problem, and the honest way around it

There is a catch about trackers: a large share of your readers never load it. **Roughly a third of web users run an ad blocker**, and on a technical blog it is routinely more than half. The default cloud scripts, served from domains like `plausible.io`, `cloud.umami.is` or `cdn.usefathom.com`, sit on the standard block lists. Those readers are simply missing from your numbers, and the more technical your audience, the more wrong your dashboard is.

The fix that actually works is a proxy. Instead of loading the script from the provider's domain, you serve it from your own, so it is indistinguishable from the rest of your site and slips past the block lists. Most providers document a proxy setup for exactly this reason.

I want to be clear about what that is for. A proxy is a way to count the readers an ad blocker would otherwise hide, not a way to quietly reinstate the tracking those readers were trying to avoid. Routing a cookie-setting, profile-building tracker through your own domain to dodge consent would be the wrong use of it. The combination worth aiming for is a cookieless provider that collects no personal data, proxied through your domain so the counts are honest. 

## Setting it up

The whole configuration is one entry in your `app.config.ts`:

```typescript [app.config.ts]
export default defineAppConfig({
  analytics: {
    providers: [
      { provider: 'hakanai', code: 'YOUR_SITE_ID' },
    ],
  },
})
```

Swap `hakanai` for whichever provider you chose and paste in the site code it gave you. The [analytics reference](https://bloggrify.com/reference/analytics) lists every supported provider and the exact code each one expects, along with the proxy setup where it applies.
