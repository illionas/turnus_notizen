# Table-Syntax Proposal

Tables are definitely a pain-in-the-ass problem in Markdown, Wiki-Syntax and the like.
There are a number of elegant ways to *display* a table in readable form. Pandoc does a great job in reducing tables to not even need the bar symbol (`|`) anymore.

However, they are all relatively difficult to type (when using whitepace, without proper text-editor) or to read (when using bars that are not aligned, or Wiki-Syntax).

Below, I props a syntax (or rather the prototype of it) that is

- easy to write and work with
- easy to understand
- accepts block elements
- not necessarily beautiful in text-only view

It is kind of a hybrid of compressed Wiki-Syntax and Pipe-Tables. In my opinion it should not matter whether you start an new cell in a new line or continue in the same line. Still, for simple tables it should be easily readable as raw text.

## Optionally put new cell on new line (with leading space)

I indeed like the idea of leaning towards the wikisyntax. One property it has is that it's relatively  easy to *write* without the help of any sophisticated editor (once you know the syntax, which looks rather difficult).

My point would be to mix in the possibility of *writing a new **cell** on a new line* into pipe tables, like this:

- `^[|].*` indicates new row (starts with pipe)
- `^\s+[|].*` indicates new cell (starts with spaces then pipe)

Or rather:

* `replace(/\n^\s+[|]/gm, "|")` -- remove newline/linefeed from lines starting with spaces before the pipe (inside identified tables)
* ***then evaluate table as usually*** → This means there would be **no big changes** to pipe tables, i guess.
* Also works with tables that lack leading or trailing pipes, since the replace rule would only match inside the rows.

```
| A1
 | B1
 | C1
 | D1 
|---|---|                       
| A2
 | B2
 | C2
 | D2
```

Until here everything about my proposal would be ***optional!***. No need to use it for small tables that are compact enough to fit into the editors view anyway. The point is that stuffed cells would get more overview.

```
| A1 | B1	| C1 | D1 |
|----|----|----|----|
| A2 | B2
    | C2: Lorem ipsum dolor sit amet, qui repudiare dissentias mediocritatem ut, quod consequat ex qui, ius in quaerendum repudiandae. Eu soleat repudiandae quo, est ullamcorper definitiones ut, cu augue sententiae quo. Ea case nihil scaevola has, eu consul propriae pro. Nec feugait corrumpit te, est ut mollis bonorum.
    | D2 |
```

You could also space out the table so that it would be easy to grasp which cell is in which *collum*, even if the cells have content of different length.

```
| A1 Lorem ipsum dolor sit amet, qui repudiare 
    | B1 dissentias mediocritatem ut, quod consequat ex qui, ius in quaerendum repudiandae. 
        | C1 Eu soleat repudiandae cu augue sententiae
            | D1 Ea case nihil scaevola has, 
|---|---|---|---
| A2 repudiare dissentias mediocritatem ut, quod 
    | B2 repudiandae quo, est ullamcorper
        | C2 est ut mollis bonorum.
            | D2 Nec feugait corrumpit 
```

## Block level elements
However, if we would change the syntax a bit it might enable us to **use block level elements inside tables!**, which would be really awsome!

```
| A1 | B1	| C1 | D1 |
|----|----|----|----|
| A2 | B2
    | - C2 ex qui
      - Lorem ipsum
      - dolor sit amet,
      - qui repudiare
    | D2 |
```

## compressed tables

I think this could be nicely combined with the idea of *compressed pipe tables* mentioned by @mofosyne with `-` or `=` to mark `<TH>`. 

```
|= A1
 |= B1
 |= C1
 |= D1             
|  A2
 |  B2
 |  C2
 |  D2
```

I personally prefer the `=` visually. Plus, maybe the combination `|-`could be used to more clearly start a new row, then you could even write a table *in one single line* like this: ` |= A1 | B1 |- A2 | B2 `. (But then the syntax would change quite a bit and the beauty of just removing leading whitespace and newline would go away.)

The `|=` might also be used to mark a `<TH>` anywhere in the table. Most common usecase might be flipped tables, where the headers are on the left, instead of on top (i.e. A1 and A2 would be headers in the examples above).

Align could be incorporated into the header marker like this: `|:=` left align, `|:=:` center align, `|=:` right align, or like @mofosyne proposed e.g. `|:= header =:` for center align.

## Rowspan

For rowspan I would suggest the "obvious" symbol for continuation, i.e. the three dots that form the "ellipsis" `...`. E.g. in the following example C2 and C3 would be merged into one cell that spans two rows.

Visually i'd propose the exclamation mark (`!`), because of it's vertical nature and the arrow-like downwards pointing of the point below it. The full-stop would probably be used to often as the last symbol and therefore generate unwanted behavior of the table. 


    |  A1  |  B1 |  C1  |  D1  |
    |------|-----|------|------|
    |  A2       ||  C2  |  D2  |
    |  A3  |  B3        |  D3  |

## Nested Tables?

are they even necessary? Should be ~~very~~ extremely rare.
Colspan and rowspan should fix nearly all of them.

```
+----+-----------+
| A1 | B1        |
+----+-----------+
| A2 |+----+----+|
|    || B2 | C2 ||
|    |+----+----+|
|    || B3 | C3 ||
|    |+----+----+|
|    | text here |
+----+-----------+
```

```
+----+-----+-----+
| A1 | B1 & C1.. |
+----+-----+-----+
| A2 |     |     |
| &  |  B2 | C2  |
+ A3 +-----+-----+
|    |  B3 | C3  |
|    |     |     |
+----+-----+-----+
```

```
| A1 | B1	| C1 | D1 |
|----|----|----|----|
| A2 | B2
    |  |  A1  |  B1 |  C1  |  D1  | <!-- C2 --> 
       |------|-----|------|------| <!-- doesn't work! -->
       |  A2       ||  C2  |  D2  |
       |  A3  |  B3 |  ... |  D3  | 
    | D2
```

# Link Syntax

Using the wikisyntax or Wiki-Creole Syntax for links i noticed, that they use the description the other way around than markdown. Obviously markdowns philosophy goes alone the lines of readability, whereas wikisyntax probably just went along the line of simplifying the `<a href="link">text</a>`
syntax.

| | |
|---------|---|
|markdown | `[text](example.com)`|
|Creole |`[[example.com|text]]`|

That made me think which one is “better” or the “right” one: i favor the text first method of markdown, because it is more readable as you can just skip the url and your eyes search for the closing symbol, whereas in creole your eyes have to find the starting symbol to be able to read the link.

That's when i thought: “Why not indicate the directionality of the link?”, i.e. do it how you want to, but indicate which part represents the url. The angle brackets `<` and `>` would be a good option for this:

	[[example.com < text]]
	[[text > example.com]]
	
	