[<- prev](first-program) | [index](../) | [next ->](understanding-structure)

## Embedding symbols inside tables

In the last lesson, we learned how to name our tables; now, we learn how to refer to tables by name.

Let's start out by reconsidering our Swahili table.  Let's use a version where we've split up the ``text" field into its meaningful parts (morphemes)


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

There's a lot of redundancy in here: every time ``ni`` occurs in the first text column, 1 apperas in the person column; every time ``na`` occurs in the second text column, present appears in the tense column, etc.  It really makes more sense to separate these correspondences out into their own tables, and then combine them.

Embedding lets us do exactly that.  We can define a symbol called ``PersonPrefix`` and another called ``TensePrefix``, and then refer to those symbols inside another table:


[]() |
|----|----|----|----|----|
| **PersonPrefix:** | **text** | **person** |
|              | ni   | 1 |
|              | u   | 2 |
|              | a   | 3 |
| &nbsp; |
| **TensePrefix:** | **text** | **tense** |
|         | na | present |
|         | li | past |
|         | me | perfect |
| &nbsp; |
| **embed** | **embed** | text | root
| PersonPrefix | TensePrefix | penda | pend

You can think of embedding PersonPrefix like putting the whole PersonPrefix table into that box.  Think of it like a command, but one with three possible results: it puts ``ni`` into text and ``1`` into person, OR it puts ``u`` into text and ``2`` into person, or ``a`` into text and ``3`` into person.

Then, in the next square, embedding TensePrefix also has three possible results, but each of those possibilities applies to all of the previous possibilities, with the result of 9 possible results.  (And that's exactly what we want, after all.  Our table above had 9 entries.)

### Let's embed the root, too

I didn't want to make things too complicated above, so I only had one root, "pend".  Let's add "ona" back in.  While we're at it, let's split off that "a", it's not really part of the root, it's just part of the verbal template.

[]() |
|----|----|----|----|----|
| **PersonPrefix:** | **text** | **person** |
|              | ni   | 1 |
|              | u   | 2 |
|              | a   | 3 |
| &nbsp; |
| **TensePrefix:** | **text** | **tense** |
|         | na | present |
|         | li | past |
|         | me | perfect |
| &nbsp;
| **Root:** | **text** | **root** |
|         | pend | pend |
|         | on | on |
| &nbsp; |
| **embed** | **embed** | **embed** | text
| PersonPrefix | TensePrefix | Root | a

### A convenient shorthand for identical columns

There's another redundancy in our table: that in the symbol ``Root``, both the text and root columns always have the same content.  It'd be nice to get rid of that redundancy, because any time there's a redundancy like this there's the possibility of human error (like updating one column and forgetting to update the other).

If we want to say that the same content should be added to *two* fields, we can just combine the field names with a slash like so:

| **Root:** | **text/root** | 
| ----------| ---------| 
|         | pend | 
|         | on |

### Templatic vs. hierarchical organization

There are two main ways we might think about verb construction in Swahili.  One is the "templatic" way: there's a template like ``PersonPrefix + TensePrefix + Root + a`` into which we stick morphemes.  That's pretty much what we did above.

However, many linguists think of complex verbs (and other complex words) being built up sequentially, like "Start with a root, then add a suffix 'a' to get a new unit, then add a TensePrefix to get a new unit, then add a PersonPrefix to get your final word". 

Sometimes it makes things easier (in the long run) to think like this, and to organize our code like this.  For example, one of these sub-units might undergo sound changes inside of it, that don't apply outside this unit.  Or, there might be interactions between morphemes that are hard to capture with the templatic structure.  (For example, circumfixes -- cases where a meaning is conveyed by simultaneously adding a particular prefix and suffix -- are hard to capture with a template.)

Here's a program with the same output as the above, but expressed hierarchically rather than templatically:


[]() |
|----|----|----|----|----|
| **Root:** | **text/root** | 
| ----------| ---------| 
|         | pend | 
|         | on |
| &nbsp; |
| **TenseStem:** | **text** | **tense** | text
|         | na | present | Root | a
|         | li | past | Root | a
|         | me | perfect | Root | a

| **Verb:** | **text** | **person** | **embed** |
|              | ni   | 1 | TenseStem |
|              | u   | 2 | TenseStem |
|              | a   | 3 | TenseStem |

We can think of this like generating all the possibilities for Root, then adding all the possibilities of tense prefixes to those to get TenseStem, then adding all the possibilities of person prefixes to those to get Verb.

You'll notice that I'm now adding that extra "a" at the same time I'm adding the tense prefix.  Why there?  Actually, this value is tied up in complicated ways with the tense prefix; it's not always "a".  (For example, for the negative past form "ku", it'll be "i".)  So handling these as part of the same table makes sense.  

(The real details get a bit complicated, so I'm not actually going to show the code that would get this right.  This isn't really a Swahili course, I just wanted to illustrate what hierarchical organization would look like.)

Which should you choose, templatic or hierarchical organization?  It really depends.  

* Templatic organization is initially a bit easier, and is probably easier for a non-linguist to wrap their heads around.  (Remember that coding isn't just communicating to a computer, it's communicating to other people!  So think about who else might be reading your code, and how they think about the language.)

* Hierarchical organization is initially a bit more complicated, but linguists incline towards it for a reason, that so many aspects of real-world languages are easier to describe when we picture them as being built-up, from the inside out, by the repeated addition of prefixes/suffixes to form intermediate units.  If your language has phenomena like this, you may want to consider making the intermediate units "real" in your code like this.

And don't be afraid to try both out!  You don't necessarily have to decide in advance and commit forever.  You can rearrange things later if you need it; programmers call this "refactoring".  It's good to stop sometimes and consider whether a different structure would express the same phenomenon in a simpler way.  Sometimes something looked like the simpler way at first, but as you added things, it got complicated, and now a different way might be simpler.

[<- prev](first-program) | [index](../) | [next ->](understanding-structure)