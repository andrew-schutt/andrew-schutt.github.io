---
layout: post
title: "Porting My Talks Off Slides.com"
date: 2026-08-31 09:00:00
categories: jekyll slides
---

I've given a handful of talks over the years and hosted every deck on [slides.com](https://slides.com/). It's a great editor, but I didn't love that all of my talks lived on a service I didn't control. So I moved them here, onto this site, where they sit in the same repo as everything else. It turned out to be pretty painless, mostly because slides.com decks are just [reveal.js](https://revealjs.com/) under the hood.

Here's the whole process.

## Getting the HTML export

The one catch is that exporting a deck as HTML requires a paid plan. I subscribed to the **Lite** tier, then for each deck used **Export → HTML**.

Each export comes down as a self-contained ZIP that has everything the deck needs to run on its own:

- `index.html` — the reveal.js deck itself, including my speaker notes and the speaker view
- `lib/` — the CSS, JS, and fonts that power the deck
- a media folder — the images and other assets

No accounts, no network calls, no build step. Just static files.

## Dropping them into Jekyll

This site is built with Jekyll, and since a reveal.js deck is just static HTML/CSS/JS, there wasn't much to do. I unzipped each export into its own folder under `slides/`, copied **verbatim**:

```
slides/
  saddle-up/
    index.html
    lib/
    <images>/
  complex-systems/
  ...
```

The important detail is that the deck's `index.html` has **no YAML front matter**. That matters because Jekyll only processes files that start with front matter — everything else is copied through byte-for-byte. So Jekyll leaves the deck completely alone, and all of its relative asset paths (`lib/…`, the image folder) resolve exactly like they did in the ZIP.

From there it was just some glue:

- A `slides/index.html` landing page — a normal Jekyll page that lists and links every talk.
- A **Slides** link in the site nav.

That's it. No plugins, no reveal.js dependency of my own to manage — each deck ships its own copy.

## Making sure nothing broke

Copying files is easy; the interesting part was confirming each deck actually still worked.

**Assets:** I checked that every image and library reference in each deck's `index.html` pointed at a file that was really there. slides.com names its image files with hashes and tucks them in a per-deck folder, so it was worth verifying rather than assuming.

**Speaker notes:** this one surprised me. My first grep for reveal.js notes came up empty, and I briefly thought the notes hadn't survived the export. They had — slides.com stores them in a JSON blob inside the page and injects them into the slides at load time, which is why they aren't visible as plain markup. The **S** key still pops open the speaker view with all my notes intact.

**One real gotcha:** one deck had `showNotes: true` in its config, which renders the speaker notes *right on the slide for every visitor*. Not what I want on a public site — notes should be for me, in the speaker view. Flipping it to `false` (matching all the other decks) fixed it while keeping the notes available behind the S key.

## Done

Nine talks are now self-hosted here, spanning 2016 through 2026 — no third-party service required. You can find them all at [/slides/](/slides/). Press **S** on any deck if you want to peek at the notes.
