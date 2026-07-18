---
layout: post
title: "I Built a Tool to Stop Wasting Time on LinkedIn Job Applications"
date: 2026-07-18
---

Here's a problem I bet you know well: you're scrolling through LinkedIn job postings, and for every single one you have to stop and ask yourself, "wait, am I actually qualified for this, or am I about to waste twenty minutes writing a cover letter for nothing?"

I got tired of doing that math in my head over and over. So I built a small tool that does it for me.

## The idea

What if, the moment I opened a job posting, something could instantly tell me: here's your match score, here's what you're strong on, here's what you're missing? No guessing, no re-reading the same paragraph three times trying to decide if "5+ years in a related field" applies to me.

I didn't want to use one of those job-matching websites either. Most of them want you to upload your CV to their servers, and honestly, I'd rather keep that information on my own computer. So I decided to build my own version: a small add-on for my browser that only I can see, that never sends my personal information anywhere.

I'm not a professional software developer. My background is in data analysis, not coding. But I figured the best way to actually learn this stuff was to just try building it, one small piece at a time, and ask for help whenever I got stuck.

## Starting from nothing

The very first thing I did wasn't write any code at all. It was just planning. What pieces does this tool actually need? A way to read the job posting off the page. A way to compare it to my own background. A way to show me the result. That was it, broken down into small enough steps that none of them felt overwhelming.

And almost immediately, I hit my first wall. I'd built most of the pieces, but forgot to actually create one of the files my own settings were pointing to. Chrome just refused to load the whole thing and complained it couldn't find something. It felt like a small, silly mistake, but that's basically the entire experience of building something for the first time. You forget one file, one connection, and nothing works until you find it.

## Making the matching actually smart

My first attempt at scoring a job against my background was really basic. It just checked whether certain words appeared in the job description or not. It technically worked, but it felt dumb. A job asking for "5+ years experience" and a job just casually mentioning "some experience helpful" would score exactly the same, which didn't feel right.

So I made it smarter in a few ways:

- If a job said "JavaScript" but my skills list said "JS," it should still count as a match. People don't always use the exact same words.
- Some of my skills matter more than others. Being great at Python matters more to me than being decent at Excel, so I wanted the tool to weigh those differently.
- Job postings usually separate "you must have this" from "nice to have, but not essential." I wanted skills mentioned in the must-have section to count for more than ones buried in a wishlist at the bottom.
- If a posting mentioned a specific number of years of experience, I wanted the tool to actually check that against my real experience, not just ignore it.

The moment I saw a real job posting get scored, with an actual percentage and a list of what I was missing, was genuinely exciting. It stopped being a bunch of code files and started being a tool I'd actually use.

## The CV problem

At first, I just typed my skills and background directly into a file in the code. That worked, but it bugged me. What if I wanted to update it? What if I accidentally shared that file with someone? My CV shouldn't live buried inside code.

So I built a proper little settings page instead, somewhere I could type in my skills, my job history, my projects, and made sure that information only ever lived inside my own browser, never anywhere else.

Then I had a bigger idea: what if I could just upload my actual CV document, and have it fill in most of those fields automatically?

This turned out to be the hardest part of the whole project, and honestly the part I'm proudest of. Browser extensions like mine aren't allowed to just grab code libraries off the internet to do the heavy lifting, for good security reasons. So I had to figure out how to read a Word document using nothing but what a web browser already knows how to do. It took some digging to understand that a Word file is secretly just a compressed folder of smaller files in disguise. Once I understood that, I could write something that carefully unpacks it and pulls the actual words out.

It's not perfect. It makes its best guess at your name, your experience, your skills, and leaves the more complicated stuff like your project history for you to fill in by hand. But watching it successfully read a real CV and fill in a form on its own, with no internet connection involved at all, felt like a genuine little breakthrough.

## Sharing it

Once it worked reliably, I didn't want to keep it to myself. I put the code somewhere safe and private online, so I have a backup and can track changes over time, and wrote simple, step-by-step instructions so my friends and family could try it out too, even if they've never touched a line of code in their life.

## What I'd tell someone starting out

If you're thinking about building something like this yourself, here's what I'd say: you don't need to understand everything before you start. I didn't. I just needed to understand the next small step. Almost every part of this project involved hitting an error message I didn't understand, asking what it meant, fixing one small thing, and moving on. That's the whole process. It's less about being a genius and more about being willing to get stuck and keep going anyway.

## What's next

I want to add a feature where, if a job scores really well, the tool can help draft a tailored version of my CV or a cover letter for it. I also want to support uploading a CV as a PDF, not just a Word document. And now that a few people are testing it, I'm looking forward to seeing where it gets things wrong, so I can make it better.