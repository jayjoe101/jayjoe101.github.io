---
title: My Second Blog Post
date: 2026-04-08 13:00:00 -0400
layout: default
image: /assets/images/thumbnails/template.png
---

<header class="mb-12">
  <h1 class="text-4xl font-bold tracking-tight leading-tight">{{ page.title }}</h1>
  
  <div class="flex items-center gap-x-4 mt-6 text-sm text-zinc-400">
    <time>{{ page.date | date: "%B %d, %Y" }}</time>
    <span class="text-zinc-500">•</span>
    <span>Written by {{ site.author }}</span>
  </div>
</header>

Hello world! 👋

This is my very first post on my brand new blog built completely from scratch with GitHub Pages + Jekyll.

You can write in **Markdown** — headings, lists, code blocks, images, everything works.

We'll make this look even better together as we go.

``` python
x = 1
```