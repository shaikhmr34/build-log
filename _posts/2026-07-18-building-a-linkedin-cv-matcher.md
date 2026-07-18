---
layout: post
title: "Building a Private LinkedIn CV Matcher (as a Complete Beginner)"
date: 2026-07-18
---

Applying to jobs on LinkedIn means reading dozens of postings and manually judging "is this actually a fit for my background?" I wanted something that could do that check automatically — scan the page, compare it against my real skills and experience, and give me a score, without sending my CV off to some third-party SaaS tool.

So I decided to build it myself: a private, local-only Chrome extension. No servers, no accounts, nothing leaving my machine. Built it step by step in VS Code with Claude, as a complete beginner to Chrome extension development.

## Why I built this

I'm not a developer by background — I work in data analysis. But I wanted a tool that fit my exact needs, and I figured the best way to learn was to just build it, one small piece at a time, and understand each part before moving to the next.

## Phase 1: Planning

Started with the boring-but-necessary part: file structure and a step-by-step roadmap, before writing any code. Landed on a simple structure — a manifest file, a background script, a content script to read the job page, a scoring engine, and a popup UI.

Rule for the whole build: one file or feature at a time, confirm it works, then move on. No giant one-shot builds.

## Phase 2: The core skeleton

Built the manifest (scoped permissions to just LinkedIn, nothing broader), a content script that scrapes the job title/company/description off the page, and a config file.

**First real bug:** I forgot to actually create the background script file, even though the manifest referenced it. Chrome refused to load the extension with a "Could not load background script" error. Lesson: manifest references need matching files — easy to miss when building file-by-file rather than all at once.

## Phase 3: The scoring engine

My first version of the matching logic was naive — just flat keyword overlap between the job description and my skills list. It worked, but felt too basic to be genuinely useful.

Rebuilt it with:
- **Skill aliases** — so "JS" matches "JavaScript"
- **Weighted skills** — core (must-have) skills score differently from bonus (nice-to-have) ones
- **Required vs bonus section detection** — splitting job descriptions into "requirements" vs "nice to have" blocks, so a skill mentioned as a hard requirement scores higher than one buried in a wishlist
- **Experience matching** — detecting "X+ years" patterns in the posting and comparing against my actual experience

## Phase 4: The popup and first live test

Built the popup UI — a button to trigger a scan, a colored score circle, and a list of matched vs missing skills.

The first live test on an actual LinkedIn job posting worked end-to-end — a real job scraped, a real score calculated, real skill gaps shown. That was the moment it went from "a pile of files" to "an actual tool."

Hit a bug along the way: "Couldn't reach the page." Turned out the LinkedIn tab was already open before the extension loaded — Chrome only injects content scripts on fresh page loads. Fix: refresh the tab.

## Phase 5: A real CV profile system

Hardcoding my CV into a config file was a bad idea long-term — awkward to edit, and not something I'd want committed to git even in a private repo.

Built an options page backed by the browser's local storage: a form for skills, titles, years of experience, summary, and projects, all stored locally and never touching the codebase or git.

## Phase 6: Auto-filling from a real CV file

This was the part I'm most proud of. I wanted to upload my actual CV (.docx) and have it auto-fill the profile form instead of typing everything by hand.

The catch: Chrome extensions (Manifest V3) block loading external script libraries from a CDN, and I couldn't get the usual document-parsing libraries installed either due to sandbox restrictions.

So I ended up writing a **document parser completely from scratch**, using only built-in browser APIs — a `.docx` file is secretly just a ZIP archive, so I wrote a small ZIP reader to find the actual document content inside it, used the browser's built-in decompression tools to unpack it, and then pulled the text out. Zero external dependencies, entirely within the extension's constraints.

On top of that, I added a best-effort guesser that reads the raw extracted text and tries to fill in fields like name, years of experience, summary, and skills — while intentionally leaving project details for manual entry, since guessing those reliably really needs an AI model, not just pattern matching.

## Phase 7: Shipping it

Set up git, created a **private** GitHub repository, pushed the first version, and tagged it. Wrote proper documentation — both a technical README and a plain-language install guide so I could share a zipped build with friends and family for testing, without them needing any developer tools.

## What's next

- An AI-powered feature to generate a tailored CV or cover letter for jobs that score highly
- Support for uploading a CV as a PDF, not just Word documents
- Refining the skill-matching accuracy based on real-world test scans

## Reflections

*(to fill in once I've had more people test it)*