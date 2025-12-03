+++
title = "A data-oriented resume"
description = "Tired of browsing Word themes?"
date = "2024-09-16"

[taxonomies]
tags = ["json", "jsonresume", "resume"]
+++

Last night it was time to set up a new resume for the _very exciting_ job market. I always end up spending 20 minutes digging through folders for my outdated Word document. Being greeted by the already-deprecated resume (and an initial attempt at battling Word's flawless layout) I began to wonder if there's a tool for this.

I don't blame Word though, it's just the only application I know that's decent at making documents. I've tried a few online editors, but they have the same problem as any other "free" product. Either it's paywalled, watermarked or extremely limited. And don't get me started on making an account for some client-side application.

# The goal

All I want as a software dev is to edit a few properties. I don't want to manually edit layouts. That's why I propose to split the resume into two parts. We have a stored object (JSON, TOML, YAML...) that contains all the information we'd want to display, and we can pass it through some other tool to generate a nice-looking resume.

Something like that already exists, though. The [JSON resume](https://jsonresume.org/) project focuses on an open-source initiative to create a JSON-based standard for resumes. There's even a [LinkedIn importer](https://github.com/joshuatz/linkedin-to-jsonresume), which is what I started out with.

Setting up my own resume only took around an hour. You can store your resume on [Gist](https://gist.github.com), and tweak your resume whenever you please.

# How's this any different to an online editor?

The difference is that we now have a [standard](https://jsonresume.org/schema) that we can build on top of. We now have a structure that any jsonresume-compliant app can work with.

The goal of that app is dependent on what you want to do with it. For example, you could serve your resume as a single HTML file and some inline CSS, or you could build an entire portfolio on top of your resume. I've seen some embedded project online where someone stored the JSON on an arduino and displayed their resume on an LCD screen?

# Alright, but I use a PDF

I've read on LinkedIn before that "links to resumes are more often than not disregarded by recruiters", whatever that means.
Nonetheless, I know recruiters typically prefer PDFs. Luckily there are enough methods to convert HTML to PDF. [Puppeteer](https://pptr.dev/) is one of them.

Here's an example I cooked up in a few minutes:

```js
import puppeteer from "puppeteer";

const browser = await puppeteer.launch();
const page = await browser.newPage();

const url = "https://registry.jsonresume.org/[your-github-username]"; // make sure you have a resume.json gist
await page.goto(url, { waitUntil: "networkidle0" });
await page.pdf({ path: "resume.pdf", format: "a4", printBackground: true });
await browser.close();
```

# Shameless self-insert

I haven't found any CLI tool that's a simple "download-and-run" deal, so I ended up implementing my own called [ferrisume](https://github.com/van-sprundel/ferrisume)(ferris + resume). If you have a bug (or maybe some critique) make sure you submit an issue.
