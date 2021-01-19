# Introduction

*The Calypso Reference* is a more formal specification on how the Calypso programming language works. This reference is inspired by the Rust reference book, which has definitely gotten me through some tough binds when working on stuff with macros.

This reference also describes the basic overview of how the bytecode compiler and VM work, and the basic structure of the project.

Right now this may be a bit unorganized and very incomplete but in the future it will probably be better.

# Notation

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