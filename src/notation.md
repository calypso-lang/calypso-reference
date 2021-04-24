# Notation

## General Writing

Calypso code will be written in code blocks like this:

```cal
fn main() {
    println("Hello, world!")
}
```

Things to look out for or further explanations of things may be provided in notes like this:
> Note: This is a note!

Or in warnings like this:

<div class="warning">

Warning: This is a warning. Oh no!

</div>

## Grammar Notation

The following grammar is used as a notation for describing the lexical grammar in various sections of this reference.

This is a slightly modified version of [*The Rust Reference*'s grammar notation][1].

| Notation          | Examples                        | Meaning                                  |
| ----------------- | ------------------------------- | ---------------------------------------- |
| CAPITAL           | HEX_DIGIT                       | A lexical category, e.g. hex digits      |
| **BoldCamelCase** | **KeywordIf**, **SintLiteral**  | A token produced by the lexer            |
| *ItalicCamelCase* | *LetStatement*, *FunctionCall*  | A syntactical production                 |
| `string`          | `x`, `while`, `*`               | The exact character(s)                   |
| \x                | \n, \r, \t, \0, \\\\            | The character represented by this escape |
| x<sup>?</sup>     | `pub`<sup>?</sup>               | An optional item                         |
| x<sup>\*</sup>    | *Stmt*<sup>*</sup>              | 0 or more of x                           |
| x<sup>+</sup>     | *Item*<sup>*</sup>              | 1 or more of x                           |
| x<sup>a..b</sup>  | HEX_DIGIT<sup>1..6</sup>        | a to b repetitions of x                  |
| /                 | *Block* / *Item*, `\n` / `\r\n` | Either one or another                    |
| { }               | {`b` `B`}                       | Any of the characters listed             |
| { - }             | {`a`-`z`}                       | Any of the characters in the range       |
| ~{ }              | ~{`b` `B`}                      | Any characters, except those listed      |
| ~`string`         | ~`\n`, ~`*/`                    | Any characters, except this sequence     |
| ( )               | (`,` *Parameter*)<sup>?</sup>   | Groups items                             |

[1]: https://doc.rust-lang.org/nightly/reference/notation.html