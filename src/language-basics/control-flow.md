Control Flow
============

Programs are typically executed one line at a time (this is called flow), but we can alter what the next line is with
control flow.

There are two main ways of doing this branching and looping.

Blocks are Expressions
----------------------

Before we get too deep into Rusts control flow I want to show you one of Rusts coolest features, expressions.

An expression in Rust is anything that could have a value. So, for example, `a + b` is an expression where we're adding
`a` to `b` which results in a value. You will also use expressions like `a == b` to compare whether the values of `a`
and `b` are the same, this results in a value of `true` or `false`.

Usually you might use an expression as part of an assignment or an evaluation, for example `let c = a + b` or 
`if a == b { ... }`, however, Rust also allows you to use a block (code between `{` and `}`) as an expression _and_ the
final value of that block can itself be an expression.

Here's a very contrived example:

```rust
#fn main() {
let c = {
    let a = result_of_a();
    let b = result_of_b();
    a + b
};
#}
```

Some cool things to note:
- `a` and `b` only exist within the code block
- the lines with `let` have semicolons
- the line with the expression `a + b` does not
- `c` will be equal to the evaluation of the code block, which itself is equal to the result of `a + b`
- the code block which `c` is equal to is also terminated with an exclamation

Why is this so cool? Because branches, loops and even functions all use code blocks!

Branching
---------

### If

The most basic form of branching is the `if` statement

```rust
#fn main() {
let expression = true;
if expression {
    println!("if expression is true print this");
}
println!("regardless of whether expression was true print this");
#}
```



### Match

- pattern matching
- can use many types
- can be cleaner than many else ifs
- can itself be an expression but all types must be the same

Looping
-------

### Loop

- no finish, only loop
- labels

### While

- exit condition in loop

### For In

- 