---
id: gs3
title: Your first Gramble program
sidebar_label: Your first program
---

[<- prev](what-is-gramble) | [index](../) | [next ->](symbol-names)

## Your first program

Okay, let's make a program that does something!  We'll start with a verb conjugator.  This isn't the only kind of program you can make with Gramble, but it's the prototypical one.

### A plain ol' CSV is a valid Gramble program already!

In the last post, we saw a little spreadsheet representing Swahili verbs.  This is actually a valid Gramble program already!  It's not an *interesting* program, but it's syntactically well-formed.  

| text  | person | tense | root |
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

The basic syntactic relationship we can see here is between a *header* (here, the headers are **text**, **person**, etc.) and content associated with those headers (**ninapenda**, **1**, etc.).  Once you click "highlight" in the Gramble menu, or "Sync & validate" in the side panel, the Gramble IDE will highlight content cells in the same color as the header they're associated with it.  

You can leave cells or whole lines blank, and the content will still be associated with the header above.  (There's a different mechanism if you want to start a second table with different headers.  Blank lines are ignored, they're not the way you start a new table.)

All of the headers here happen to represent field names in the abstract database that this program represents, but that won't always be true once we start writing more complex programs.

### An important difference between Gramble programs and database tables

Here's an important illustration that headers and field names aren't always the same thing.  You'll want to understand this before moving on.  The following table results in exactly the same outputs as the previous table. 

| text | text | text | person | tense | root |
|----|----|-------|--------|-------|-----|
| ni | na | penda | 1 | present | pend |
| u  | na | penda | 2 | present |  pend |
| a  | na | penda | 3 | present |  pend |
| ni | li | penda | 1 | past |  pend |
| u  | li | penda | 2 | past |  pend |
| a  | li | penda | 3 | past | pend |
| ni | me | penda | 1 | perfect | pend |
| u  | me | penda | 2 | perfect | pend |
| a  | me | penda | 3 | perfect | pend |

The outputs of this are exactly the same as the above, it results in ``{text: ninapenda, person: 1, tense: present, root: pend}, {text: unapenda, person: 2, ...}, etc.}``.  There's still just one ``text`` field, even though there are three ``text`` headers.

So this is an important distinction between a Gramble program and a database table.  The association between ``text`` and ``ni`` here isn't a declaration that the value of the **text** field is ``ni``.  Rather, you can think of this more like an instruction: "Add ``ni`` to the text field!"  We can perform operations multiple times to the same field, whereas a database can't have multiple fields with the same name.

### What restrictions are there for me to use a database table as a Gramble program, or in a Gramble program?

Basically, if your field names are plain text and don't contain spaces, you're probably fine.

More specifically, field names should NOT:

* contain spaces
* start with a special symbol like "%" or ">" 
* contain a forward slash "/"
* end in a colon
* be a [reserved word](../reference/reserved-words) like "embed" or "table".

### Do I have to call fields "text", etc.?

No, there's nothing special about the words "text", "person", etc.  Gramble doesn't have any pre-defined field names, these could be "dfaswekjlasdl" so far as Gramble cares.

The client program (the program that makes queries of a Gramble database, for example a verb conjugator webpage) might have certain requirements about what fields are named. E.g., if the client is making requests like ``{person: 1, tense: perfect}``, then the database should have those as fields, or else it'll never be able to return any results matching them!  But Gramble itself doesn't know or care what "person" or "tense" means.

It's up to you to make sure that the client program and database have the same expectations about what fields are named.  (By the way, there's a way to [rename](../reference/remane) fields, so that you can use the same Gramble program with multiple client program even if those clients disagree about what things should be called.)

[<- prev](what-is-gramble) | [index](../) | [next ->](symbol-names)