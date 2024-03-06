---
title: 'Coming Back to Poker - Part 1'
description: 'The Plan for a new and improved Poker Game'
pubDate: 'Aug 20 2023'
heroImage: '/blog-placeholder-4.jpg'
---

## The plan

I wanted to really take my time with this project and write some elegant and reliable code, so came up with a list of ideals to try to stick to.

## The Manifesto

1. End to End Type safety: this is probably the most important one for me. After far too many hours of my life spent debugging questionable JavaScript code, I wanted something a bit more robust, so went with TypeScript for both the frontend and backend.
2. Lightweight
3. Well-tested
4. Functional, as much as is reasonably possible.

## Starting off

The first thing I started with was some initial data modelling. Before thinking about Cards, I first defined some types for Players and Roles. "What do you mean by role?" you might ask. This is essentially a way to keep track of what the player is doing gameplay-wise.