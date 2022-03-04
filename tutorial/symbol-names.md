[<- prev](first-program) | [index](../) | [next ->](understanding-structure)

## Symbol names

### Naming a table

It'll be useful later on to be able to refer to a table by name.  (Actually, if this is the only thing on the whole worksheet, or the last thing on a worksheet, it already has a name, you can refer to it by the name of the worksheet itself.  But that aside...)

To name a table, shift it to the right and add a name like "SymbolName:", with a colon at the end.

| Verb: | text | person | tense | root |
|--|--------|--------|-------|-----|
| | ninapenda | 1 | present | pend |
| | unapenda | 2 | present |  pend |
| | anapenda | 3 | present |  pend |

If you sync & validate in the Gramble Control Panel, you'll see that there are now two symbols you can query/generate/sample from: ``YourSheetName``, and ``YourSheetName.Verb``.  (But these both happen to refer to the same table, like I said, the name of the worksheet refers to the last thing in the worksheet, and the last thing on this sheet is ``Verb``.)

Names follow similar rules to field names: don't use spaces, don't start them with special symbols, etc.  (But *do* end them with colons, this time!  Otherwise Gramble will think it's a header and get confused.)

### You can have multiple named tables in one worksheet

Say we wanted to have separate tables for verbs and nouns.  That's a good idea, because verbs have fields that are irrelevant to nouns (like tense) and nouns have fields that are irrelevant to verbs (like class).

We could put these in separate sheets, but we can also put them in the same sheet so long as they have names.

| Verb: | text | person | tense | root |
|--|--------|--------|-------|-----|
| | ninapenda | 1 | present | pend |
| | unapenda | 2 | present |  pend |
| | anapenda | 3 | present |  pend |
| |
**Noun:** | **text** | **class** | **root**
| | mtu | 1 | tu
| | watu | 2 | tu
| | mtoto | 1 | toto
| | watoto | 2 | toto

If you sync & validate in the Gramble Control Panel, you'll see that there are now *three* symbols you can query/generate/sample from: ``YourSheetName``, ``YourSheetName.Verb``, and ``YourSheetName.Noun``.  ``YourSheetName`` alone now refers to the same table as ``YourSheetName.Noun``, because that's the last-defined table on the page.

(Actually, you don't necessarily have to name the last table, since it's going to have a name anyway.  But we usually do anyway.  It's nice to keep similar content in similar columns, rather than have the last one shifted over like that.  Speaking of which...)

### Marking tables with "table:"

If you're reading other people's Gramble code, you'll often see another operation in here, "table:", in between the symbol name and the table itself.  This operation doesn't actually do anything here, but "table:" can help with the overall structure of more complex programs.  (We'll see how in the next lesson.)

| Verb: | table: | text | person | tense | root |
|--|--|------|--------|-------|-----|
| | | ninapenda | 1 | present | pend |
| | | unapenda | 2 | present |  pend |
| | | anapenda | 3 | present |  pend |

[<- prev](first-program) | [index](../) | [next ->](understanding-structure)