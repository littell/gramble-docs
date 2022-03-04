---
id: gs3
title: Your first Gramble program
sidebar_label: Your first program
---

## Your first program

Okay, let's make a program that does something!  We'll start with a verb conjugator.  This isn't the only kind of program you can make with Gramble, but it's the prototypical one.

### A plain ol' CSV is a valid Gramble program already!

In the last post, we saw a little spreadsheet representing Swahili verbs.  This is actually a valid Gramble program already!  It's not an *interesting* program, but it's syntactically valid.  

| verb  | person | tense | root |
|-----------|--------|-------|-----|
| ninapenda | 1 | present | pend |
| unapenda | 2 | present |  pend |
| anapenda | 3 | present |  pend |
| nilipenda | 1 | past |  pend |
| ulipenda | 2 | past |  pend |
| alipenda | 3 | past | pend |
| nimependa | 1 | perfect | pend |
| umependa | 2 | perfect | pend |
| amependa | 3 | perfect | pend |

You can copy and paste this table into your Google Sheet and the Gramble IDE will understand it and be able to query it, sample from it, etc.
