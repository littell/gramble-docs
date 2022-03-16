[<- prev](testing) | [index](../) | [next ->](../)

## Understanding the structure of Gramble programs

Our programs are starting to get structurally complicated, so it's time to get specific about how their syntax is interpreted.  The syntax of Gramble isn't all that difficult, but it's very different from other languages, so it's important to understand the basics.

To start off, consider what the Gramble interpreter has to do with the programs in the previous chapter.  It's got a big grid of cells with text in them.  Some of those cells contain actual pieces of linguistic content like "penda" or "1".  Others are names of fields like "text" or "person".  Others are things like symbol names ("Verb:") and commands that do various things ("table:").

The Gramble interpreter has to be able to go through each cell in turn and make a decision, "how am I going to interpret this cell"?  It can only do that if we all agree on some rules about how cells are to be interpreted.  So you have to know those rules, too, so that you can be sure that you and the Gramble interpreter agree about the interpretation.  (And for the most part, when you make a mistake, it's clear from highlighting and error messages.  If you click "Highlight" or "Sync & validate", and the highlighting is something totally different from what you expected, something is wrong.)

### The table-parsing algorithm

The Gramble interpreter visits every cell once, working left to right along each row, and then down the rows.  (Exactly like you're reading this document.)  In every cell, it makes a decision about what a cell means, and that decision is final; it never backs up or changes its mind.

### Comments

First off, if the row is entirely empty, or if the first cell starts with "%%", the row is ignored completely, and the interpreter moves on to the next row.  A row starting with "%%" is a [comment](../reference/comments), you can use it to write notes to yourself and others, or temporarily "turn off" a line that you don't want to be interpreted.  Note that "%%" only works in the first column; a "%%" elsewhere will not create a line comment, it'll just be interpreted as being "%%".

### Blocks

Next, there's a concept called a "block", which is a rectangular region of cells that can be interpreted as a unit.  Blocks are typically created by cells ending in ":", like "Verb:" or "table:".  Everything to the right of that "block-started" cell is considered "in the block", and everything in subsequent rows is also "in the block" until there is text in a cell directly under the block-starter, or under-and-to-the-left.  (If you're familiar with Python, it's basically the "offside rule".)

This is much easier seen than described; in the Gramble worksheet below, there are four blocks.  One is started by "Verb:", which contains everything that follows until "Noun:".  One is started by "table:", which also contains everything that follows until "Noun:".  (Blocks can nest; the "table:" block is inside the "Verb:" block.)  Next, there's the "Noun:" block; it ends both previous blocks because it's below or below-and-to-the-left of their respective block-starters.  It continues until the end of the page.  Finally, there's another "table:" block, inside the "Noun:" block, that continues until the end.

| Verb: | table: | text | person | tense | root |
|--|---|-----|--------|-------|-----|
| | | ninapenda | 1 | present | pend |
| | | unapenda | 2 | present |  pend |
| | | anapenda | 3 | present |  pend |
| |
**Noun:** | **table:** | **text** | **class** | **root**
| | | mtu | 1 | tu
| | | watu | 2 | tu
| | | mtoto | 1 | toto
| | | watoto | 2 | toto

But what if one of our words has a ":" at the end?  There are some languages that use ":" as part of their writing system!  There's also a rule about block-starters that, once the interpreter finds one cell that *isn't* a block-starter (like the header "text" in the first row above), you can't start another block until the current block has been closed.  We could put a ":" anywhere in those tables and it wouldn't be interpreted as a block-starter.  It's only when the block ends (here, "Noun:" breaks the block above it by its position) that ":" will be interpreted as starting a block again.

By the way, every worksheet is considered a block on its own; think of it as having an implicit, invisible block-starter beyond its upper-left corner.  The name of this block, as mentioned in the [symbol names](symbol-names) lesson, will be the name of the worksheet as a whole.

### Headers

After a block-starter, the interpreter moves on to the next cell.  If this cell ends in ":", then it's another block-starter.  If it does *not* end in ":", then the cell begins a header row.  This cell, and any cell to its right, will be interpreted as a header.

Since (as mentioned above) every worksheet is itself a block, if the first cell the interpreter encounters doesn't end in ":", it's considered a header.  (That's why it's possible to just have a plain CSV database interpreted as Gramble source.)

### Content

All cells directly under header cells are considered "content" -- generally speaking, they'll represent linguistic information like "ninapenda" or "1" -- until the block ends.  

By default, all content cells are interpreted *literally*, which means that any characters (letters, numbers, etc.) inside them will be interpreted as referring only to those characters.  For example, we learned earlier that "%" and ":" have special interpretations in Gramble, but inside a content cell they have no special interpretation, they're just the literal characters "%" and ":".

The reason for this is that special characters are a perennial stumbling block for new programmers (and non-programmer subject-matter experts) working on projects like these.  An experienced programmer knows to be aware of special characters; all you have to do is tell them "These characters are special; to use them literally you have to escape them."  (Escaping means putting something before the character, usually "\", to turn off its special interpretation.)  However, a new programmer or non-programmer won't have any preconceived notions of which characters are special, that they exist at all, what escaping is, that you sometimes have to escape the escape character, etc.

Gramble projects are assumed from the outset to be multi-specialty collaborations.  We want the non-programmer collaborators to feel confident about entering their knowledge into the worksheets, without fear that they're going to break the whole thing.  Special characters are a classic example of something non-programmers don't know about that can cause errors, and unlike a lot of Gramble syntax you can't learn them by analogy.  So that's why, by default, we considered special character interpretations "turned off" inside content cells.

By the way, if you *do* want regex-like special characters inside cells, you can turn that on with a [special header](../reference/regexes).

### Everything else

If you've got a cell that *can't* be interpreted as a comment, block-starter, header, or content (for example, a content cell hanging out on its own without any header cell above it), then that's an error, and will be highlighted in bright red to call your attention to it.

Although it might feel demoralizing to see a page full of red sometimes, error messages are your best friend in the long run.  Really, when we ask Gramble programmers why they prefer working with it compared to other toolkits, a common answer is "It's so much better at spotting errors, and giving me meaningful error messages."  Discovering errors in your code early will save you many hours of frustration later on.

[<- prev](testing) | [index](../) | [next ->](../)