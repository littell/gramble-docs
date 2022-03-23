[<- prev](embedding) | [index](../) | [next ->](understanding-structure)

## Testing your program

It's important to periodically make sure your program is doing what it's supposed to.  But as the program grows, the generation/sampling buttons won't be enough.  You might be interested in one particular form, but your program generates a million forms, you don't want to keep hitting the sample button until you find it, or generate a million forms and search for it.  Or, you might be so interested in one form, you miss that you introduced an error that made a thousand other forms wrong!

So we need a way of testing the units of our program ("unit testing") to make sure they have the outputs we expect.

There's a command "test:" where we can say, "I expect this program to be able to generate these forms."

[]() |
|----|----|----|----|----|
| **Root:** | **table:** | **text/root** | 
|       |  | pend | 
|         | | on |
| &nbsp; |
| **TenseStem:** | **table:** | **text** | **tense** | text
|         | | na | present | Root | a
|         | | li | past | Root | a
|         | | me | perfect | Root | a

| **Verb:** | **table:** | **text** | **person** | **embed** |
|            |  | ni   | 1 | TenseStem |
|            |  | u   | 2 | TenseStem |
|            |  | a   | 3 | TenseStem |
| &nbsp;           | 
|           | **test:** | text  | person | tense | root |
| | | ninapenda | 1 | present | pend |
| | | ulipenda | 2 | past |  pend |
| | | umependa | 2 | perfect | pend |
| | | amependa | 3 | perfect | pend |

You'll notice that I added the (otherwise meaningless) "table:" command above "test:"; that's structurally necessary to prevent "test:" from being misinterpreted, for reasons you'll learn in the next lesson.  (I also added it to the other symbols, too.  It's not necessary there, I just like to keep things lined up.)

Im the Gramble menu at the top of the screen, there's a command "Run unit tests".  Click this, and it will check whether each of these test forms can be generated.  If it successfully generates the form with all the correct field values, the leftmost cell of the test line (e.g. ``ninapenda``) will be highlighted in green.  If it cannot generate that form with all the correct values, the leftmost cell will be highlighted in red.

By the way, there's nothing special about that particular cell that gets turned green/red.  The highlighting just has to go somewhere in that row, and the leftmost cell is a convenient place.  It doesn't mean that that's the erroneous part.  (The system generally isn't able to "introspect" to know why it can't make a particular form; all it knows is that that form isn't among the possible outputs.)

### Testing and refactoring

Having tests like this is especially important when you're changing and rearranging code.  It is *very* easy to inadvertantly change something that makes a difference in the outputs, and not notice it.  

So when you're refactoring, you want to make sure you have unit tests that capture what you care about, and re-run those tests frequently.  The cycle should be:

* Make a small structural change that you *think* shouldn't change the results
* Re-run your unit tests to help confirm to yourself that nothing changed
* Repeat

The amount of assurance you can get that nothing changed is, of course, dependent on how thorough your test cases are!  

### Testing different hierarchical levels

These tests don't have to be at the "end" of the program, testing whole words.  If you've got an intermediate unit that you care about, go ahead and add tests there:

[]() |
|----|----|----|----|----|
| **Root:** | **table:** | **text/root** | 
|       |  | pend | 
|         | | on |
| &nbsp; |
| **TenseStem:** | **table:** | **text** | **tense** | text
|         | | na | present | Root | a
|         | | li | past | Root | a
|         | | me | perfect | Root | a
| &nbsp;           | 
|           | **test:** | text | tense | root |
| | | napenda | present | pend |
| | | lipenda | past |  pend |
| | | naona | present | on |
| | | meona | perfect | on |

| **Verb:** | **table:** | **text** | **person** | **embed** |
|            |  | ni   | 1 | TenseStem |
|            |  | u   | 2 | TenseStem |
|            |  | a   | 3 | TenseStem |

This is another reason many Gramble programmers default to putting that "table:" command on every table, it means that any table is ready for future tests without having to shift it over later.

Also, make sure you're adding your tests to the correct symbol!  If you were to add these tests under ``Verb:`` instead, they would all fail, or if you added the ``Verb:`` tests to ``TenseStem:`` instead, the tests would fail.  Tests test whether a specific symbol can generate each output, not whether the program as a whole can.

[<- prev](embedding) | [index](../) | [next ->](understanding-structure)
