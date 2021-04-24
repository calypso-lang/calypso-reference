# Notes

These are notes to myself for a later date so I don't forget the design choices I made. I'm bad at remembering things.
If there's anything here that looks like bad design, don't fret to note it out and suggest changes.

## Notation

`<X$name>DY` where `Y` is a regex quantifier (e.g. `*`, `+`, `{m, n}`) and `D` is a delimiter implies that `X` is repeated as many times as `Y` specifies, delimited by `D`.

## Functions, functions, functions

### Definition

```cal
fn NAME(<IDENT$name: TY$type <\\ EXPR$default>?>,*<,>?)<: TY$return_type>? -> EXPR
```

Remember that `do end` blocks are expressions.

### Calling

```cal
<PATH>.*NAME(<EXPR$arg <OR> _ /* (for partial application) */>,*<,>?)
```

### Function Pointer/Closure Type

I don't think I'll need a different closure type like `Fn`/`FnMut`/`FnOnce` for Rust, as I'm pretty sure that mostly has to do with lifetime stuff.

```cal
fn(<TY$type>)<: TY$return_type>?
```

### Overloading

You can have functions of different arities, but NOT of different argument/return types.

Default arguments implicitly desugar to versions of the function until there are no more default arguments. E.g.:
```cal
fn foo(a, b, c \\ x, d \\ y) -> BODY
desugars to
fn foo(a, b, c \\ x) -> foo(a, b, c, y)
and
fn foo(a, b) -> foo(a, b, x, y)
```

Thus, foo/4 with 2 default params desugars into foo/3 and foo/2. The compiler will yell at you (in the form of an error, of course, but I love to entertain the thought of actually playing a sound at max volume of text-to-speech yelling) if you already have a function foo/3 or foo/2 in this case, as there's no way to tell if you wanted the version of foo/4 with default params or the other implementation.

## Pattern matching and result types

Pretty much just ~~stolen~~ (*ahem* "borrowed" *ahem*) from Rust.

### Overall syntax: `case` and `with`

```cal
case EXPR$scrutinee do
    <PATTERN$case -> EXPR>,+<,>?
end
```

```cal
with <PATTERN$case <- EXPR$scrutinee <OR> BOOLEXPR$cond>,+<,>? -> EXPR <else -> EXPR>?
```

ex:
```cal
with some_idx != 0,
     Some(elem) <- some_list.get(some_idx),
     Ok(res) <- some_io_op(elem) ->
    do_some_thing_with(res)
else ->
    Err("could not blahblahblah")
```

### `with` expr type semantics

type of the with expr:
- success case:
  - return type of success expression
- failure case:
  - if else block is present, return type of else block
  - if else block is not present, and
    - all scrutinees are the same type, the type of the scrutinee
    - not all scrutinees are the same type, `()`
    - all scrutinees are simple boolean checks, `bool`
- success and failure cases must have the same type
- if there is no else block and the success type and failure type are different, `()`
- if there is an else block and the success type and failure type are different, type error

### Pattern syntax

- `IDENT`: match anything and bind to variable with that name
- `_`: wildcard: match anything and don't care about value
- `A | B`: match A or B. must have same binding on either side.
- `CONSTEXPR`: match constant expression.
- `(<PATTERN>,+<,>?)`: tuple
- `IDENT$struct_name { <IDENT$field: PATTERN>,*<,>? <.. /* ignore rest */>? }`: struct
- `IDENT$variant(<PATTERN>,+<,>?)`: tuple variant
- `IDENT$variant { <IDENT$field: PATTERN>,*<,>? <.. /* ignore rest */>? }`: struct variant
- `[<PATTERN>,+<,>? <.. /* ignore rest */>?]`: list
- `[<PATTERN>,+<,>? | IDENT$tail]`: list, linked-style pattern matching. boils down to iterators/push-pop/slices, probably (cause linked lists are inefficient)
