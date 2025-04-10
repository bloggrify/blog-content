---
id: "4"
author: "hlassiege"
title: "Announcing Bloggrify 2.0: Enhanced SEO, Analytics, and Developer Experience"
description: "Major release with improved SEO capabilities, multiple analytics providers, and better developer experience. Over 46 commits bringing significant improvements to the framework."
date: "2025-01-24"
tags:
  - release

---

We're excited to announce the release of Bloggrify 2.0.0, representing a lot of development and 46 commits. This release focuses on improving SEO capabilities, expanding analytics options, and enhancing the developer experience.

## Major Improvements

### Enhanced SEO Stack
- Integrated `nuxtjs/robots` and `nuxtjs/sitemap` modules
- Added Schema.org support with BlogPosting annotations
- Enhanced meta tags and OpenGraph support
- Report reading time in meta tags and schema.org

### Analytics Support
Multiple provider support for web analytics:
- Hakanai integration
- Fathom Analytics
- Google Analytics
- Support for multiple providers simultaneously

### Developer Experience
- Improved error reporting during build
- Simplified template writing
- Better handling of internal links
- Enhanced documentation

### Accessibility Improvements
- Improved contrast ratios
- Added ARIA labels
- Semantic HTML enhancements
- Table of contents using proper nav elements

## Breaking Changes
If upgrading from 1.x, as a user, impacts are minimal. However, theme developers should be aware of deprecations and changes.

The migration guide is available in the [documentation](https://bloggrify.com/introduction/migration)


Full [changelog available on GitHub](https://github.com/bloggrify/bloggrify/releases/tag/v2.0.0).


## Contribute
Bloggrify is open source. [Help us on GitHub](https://github.com/bloggrify/bloggrify):

* Report issues
* Submit PRs
* Join discussions
