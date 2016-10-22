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

| A1 | B1 | C1 | D1 |
|--|--|--|--|
| A2 | B2 | C2 | D2 |

Someone already suggested a compressed pip-table syntax, similar to this:

```
|= A1	|= B1 |= C1 |= D1 |
|  A2	|  B2 |  C2 |  D2 |
```

So my idea is that you can start the new cells in new lines if needed and optionally space them out so that the location of the bar visually indicates which column it belongs to. 

```
|= A1
    |= B1
        |= C1
            |= D1						
|  A2
    |  B2
        |  C2
			
          Some additional paragraph for C2
			
            |  D2

```

Probably the amount of whitespace in front wouldn't matter a lot:

- `[bar].*` indicates new line
- `\s*[bar].*` indicates new cell

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

It could be backwards-compatible to normal pipe-tables (with or without line-breaks).

```
| A1 | B1
	| C1 | D1 |
|----|----
	|----|----|
| A2 | B2
	| C2 | D2 |
```