---
pageid: "9"
title: "About"
description: "About the official Bloggrify blog."
date: "2024-02-11"
listed: false
nocomments: true
categories:
    - "documentation"
tags:
    - "bloggrify"
    - "documentation"

schemaOrg:
    "@type": "AboutPage"
---

# About this blog

This is the official blog of [Bloggrify](https://bloggrify.com), a static blog generator built on
Nuxt Content. Release notes, new features and writing guides are published here.

It also doubles as the live demo of `minimalist`, the theme shipped with Bloggrify by default:
what you are reading is exactly what you get out of the box.


## Getting started

First, you need to create a new Nuxt application. You can do this by running the following command:

```bash
npx nuxi@latest init myblog
```

Then, you need to install the dependencies:

```bash
npm install @bloggrify/core
npm install -D sass-embedded
```

Then you have to explicitly say to Nuxt that you are using Bloggrify as an extended module. You can do this by adding the following line in your `nuxt.config.js` file:

```json
    extends: [
        '@bloggrify/core',
    ],
```

## Create a basic configuration file

You should have a default configuration file in the root of your project: `app.config.ts`. The file is however empty. You can read more about the configuration options [here](https://bloggrify.com/introduction/configuration).

Then you can run the development server on http://localhost:3000

```bash
npm run dev
```

Next, you can remove all contents from the `content` folder and start from scratch and create your own [content](https://bloggrify.com/introduction/writing-pages).
