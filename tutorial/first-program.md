---
id: gs3
title: Your first Gramble program
sidebar_label: Your first program
---

[<- prev](what-is-gramble) | [index](../) | [next ->](understanding-structure)

## Your first program

Okay, let's make a program that does something!  We'll start with a verb conjugator.  This isn't the only kind of program you can make with Gramble, but it's the prototypical one.

### A plain ol' CSV is a valid Gramble program already!

In the last post, we saw a little spreadsheet representing Swahili verbs.  This is actually a valid Gramble program already!  It's not an *interesting* program, but it's syntactically valid.  

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

The basic syntactic relationship we can see here is between a *header* (here, the headers are **text**, **person**, etc.) and content associated with those headers (**ninapenda**, **1**, etc.).  All of the headers here represent field names in the abstract database that this program represents, but that won't always be true.

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

The outputs of this are exactly the same as the above, it results in ``{text: ninapenda, person: 1, tense: present, root: pend}, {text: unapenda, person: 2, ...}, etc.}``.  There aren't three different text fields now, there's still just one.

So this is an important distinction between a Gramble program and a database table.  The association between **text** and **ni** here isn't a declaration that the value of the **text** field is "ni".  Rather, you can think of this more like an instruction: "Add 'ni' to the text field!"  We can perform operations multiple times to the same field, whereas a database can't have multiple fields with the same name.

### What restrictions are there, on a database table, for me to use it intact as a Gramble program, or in a Gramble program?

Basically, if your field names are plain text and don't contain spaces, you're probably fine.

More specifically, field names should NOT:

* contain spaces
* start with a special symbol like "%" or ">" 
* contain a forward slash "/"
* end in a colon
* be a [reserved word](../reference/reserved-words) like "embed" or "table".

### Naming a table

It'll be useful later on to be able to refer to this table by name.  (Actually, if this is the only thing on the whole worksheet, or the last thing on a worksheet, it already has a name, you can refer to it by the name of the worksheet itself.  But that aside...)

To name a table, shift it to the right and add a name like "SymbolName:", with a colon at the end.

Verb: | text | person | tense | root |
|--|--------|--------|-------|-----|
| | ninapenda | 1 | present | pend |
| | unapenda | 2 | present |  pend |
| | anapenda | 3 | present |  pend |

If you're reading other people's Gramble code, you'll often see another operation in here, "table:", in between the symbol name and the table.  This operation doesn't actually do anything here, but "table:" can help with the overall structure of more complex programs.  (We'll see how in the next lesson.)

Verb: | table: | text text | person | tense | root |
|--|--|------|--------|-------|-----|
| | | ninapenda | 1 | present | pend |
| | | unapenda | 2 | present |  pend |
| | | anapenda | 3 | present |  pend |


[<- prev](what-is-gramble) | [index](../) | [next ->](understanding-structure)