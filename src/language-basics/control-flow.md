Control Flow
============

Programs are typically executed one line at a time (this is called flow), but we can alter what the next line is with
control flow.

There are two main ways of doing this [branching](#branching) and [looping](#looping).

Before we do that though, lets talk about two of Rusts coolest features, which will come up a lot later, 
[patterns](#patterns) and how [blocks are also expressions](#blocks-are-expressions).

Patterns
--------

In the last chapter we talked about compound types. Tuples, Structs, and Enums allow the construction of more complex
data from less complex data. However, if we want to extract any of the component parts of that data we can do that!

Patterns can be used to extract the data from tuples is pretty trivial:

```rust
#fn main() {
let point = (123, 456);
let (x, y) = point;
println!("The point was at x: {x} and y: {y}");
#}
```

It's important to note though, that the original data will no longer be accessible:

```rust
#fn main() {
// TODO: Test this, might need something that isn't Copy!
let point = (123, 456)
let (x, y) = point;
let this_wont_work = point.0;
#}
```

This pattern also works for Tuple Structs, however, you need to specify the name of the struct like you're doing a weird
backwards struct instantiation. 

```rust
struct Point (u64, u64);

fn main() {
    let point = Point(123, 123);
    
    let Point(x, y) = point;
    
    println!("The point was at x: {x} and y: {y}");
}
```

The same thing also works for Structs with Named Fields:

```rust
struct Point {
    x: u64,
    y: u64,
}

fn main() {
    let point = Point { x: 123, y: 123 };
    
    let Point { x, y } = point;
    
    println!("The point was at x: {x} and y: {y}");
}
```

In the above example we extract the structs named fields straight into variables of the same name as its easy and the
names were appropriate. However, it might be better in the context of your program to name them something else. Below
we've renamed `x` to `width` and `y` to `height`:

```rust
#struct Point {
#    x: u64,
#    y: u64,
#}
#
#fn main() {
#    let rect = Point { x: 123, y: 123 };
#
let Point { x: width, y: height } = rect;

println!("The rect was {width} wide and {height} high");
#}
```

Unfortunately, you can not extract data from Enums this way as the value of an Enum is one of a set of, not only values,
but potentially subtypes or shapes or however you'd like to describe them. Take for example the humble Options:

```rust
#fn main() {
let maybe_yuki: Option<char> = Option('雪');
let maybe_not: Option<char> = None;
#}
```

How can we extract a `char` from `Option<char>` if we don't know whether the variable is `Some` or `None`... well, 
actually, we'll come to that soon. 🙂

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

The most basic form of branching is the `if` statement.

In its most simple form it's an `if` followed by an expression (unlike many languages this does not need to be in 
brackets) followed by a code block. The expression must evaluate to a boolean, either `true` or `false`. If the
expression evaluates to `true`, then the code in the block will be run, otherwise it won't be: 

```text
if <expression> {
    <code to run if expression is true>
}
```

For example, we could create an expression that evaluates to a boolean by comparing if two numbers are the same, using
double equals:

```rust
#fn main() {
let a = 1;
let b = 1;
    
if a == b {
    println!("if expression is true print this");
}
println!("regardless of whether expression was true print this");
#}
```

If you want to run some code if the expression is `true`, but some different code if its `false`, then you can extend
`if` with `else`. Here we compare if the first number is greater than the second number.

```rust
#fn main() {
let a = 1;
let b = 1;
    
if a > b {
    println!("if expression is true print this");
} else {
    println!("if expression is false print this instead");
}
#}
```

You can chain `if`/`else` statements to create more complex branches.

```rust
#fn main() {
let a = 1;
let b = 1;
    
if a > b {
    println!("a is greater than b");
} else if a == b {
    println!("a is equal to b");
} else {
    println!("a must be less than b");
}
#}
```

Remember though, code blocks, including those in `if` and `else` are themselves expressions. This means they can 
effectively return their own values

```rust
#fn main() {
let a = 1;
let b = 1;
    
let message = if a > b {
    "a is greater than b".to_string() 
} else if a == b {
    "a is equal to b".to_string()
} else {
    "a must be less than b".to_string()
};
    
println!("{message}");
#}
```

Some important things to note:
1. The last line of each code block has no semicolon
2. When we create expressions like this, we must terminate them with a semicolon (see after the final `}`)
3. All branches must evaluate to the same _Type_, even if they don't evaluate to the same _value_
4. Doing big blocks of `if`/`else if`/`else` is a mess, there's a [better way](#match)!

#### Pattern Matching inside `if` and `else`

There is another way you can branch with `if` that doesn't require a boolean expression, pattern matching.

There are two ways to do this `if let ...` and `let ... else`.

Let's go back to that Option from earlier:

```rust
#fn main() {
let maybe_yuki: Option<char> = Option('雪');
let maybe_not: Option<char> = None;
    
if let Some(c) = maybe_yuki {
    // This line will be printed
    println!("The character was '{}'", c);
}
    
if let Some(c) = maybe_not {
    // This line will not
    println!("The character was '{}'", c);
}
#}
```

In the line `if let Some(c) = maybe_yuki` we are pattern matching on the Option, if it matches the pattern of 
`Some(<variable>)`, then we extract the contents of the `Some` into the `<variable>`. Within the block (and only within
the block), the variable `c` has the value from inside the `Some` variant of the `Option`.

This may be easier to observe with our own enum type. Imagine the following:

```rust
enum Vector {
    Two(usize, usize),
    Three(usize, usize, usize),
}

fn main() {
    let v = Vector::Three(3, 4, 5);
    
    if let Vector::Two(x, y) = v {
        // This line will not be printed
        println!("The 2D vector has the magnitude '{}'", x * y);
    }
    
    if let Vector::Three(x, y, z) = v {
        // This line will
        println!("The 3D vector has the magnitude '{}'", x * y * z);
    }
}
```

This example is a little contrived, there are better ways to do this.

You can also do the opposite, branch if the pattern does not match, using `let ... else`. The important thing to note
here is that execution can not continue after the code block, you must exit the current flow, whether thats returning 
from a function or breaking from a loop

```rust
fn some_function() {
    let maybe_yuki: Option<char> = Option('雪');
        
    let Some(c) = maybe_yuki else {
        // This code is executed if the maybe_yuki was None
        // We must exit from the code here, as we can not go back to the normal execution
        return;
    };
    // From this point forward, the contents of the Option has been extracted into the variable `c`
    println!("The character was '{}'", c);
}
```

### Match

This pattern matching stuff is really handy, right?!

Well the creators of Rust thought so too, in fact, they made a whole control flow mechanism around it!

`match` is a bit like `if` in that it can branch, and act as an expression. However, `match` can do a lot more than
`if`, it will match against multiple possibilities, allows match guards for fine grain control of pattern matching, and
its exhaustive, meaning that a match _must_ deal with every possibility.

Lets look at our Vector example again:

```rust
enum Vector {
    Two(usize, usize),
    Three(usize, usize, usize),
}

fn main() {
    let v = Vector::Three(3, 4, 5);
    
    match v {
        Vector::Two(x, y) => println!("The 2D vector has the magnitude '{}'", x * y),
        Vector::Three(x, y, z) => println!("The 3D vector has the magnitude '{}'", x * y * z),
    }
}
```

First of all, you can see that this pattern is _much_ cleaner than having a lot of `if let`s. We're matching against
the variants of an enum, and can immediately extract the contents from each variant. We could also use match as an
expression:

```rust
#enum Vector {
#    Two(usize, usize),
#    Three(usize, usize, usize),
#}
#
#fn main() {
#    let v = Vector::Three(3, 4, 5);
#    
let magnitude = match v {
    Vector::Two(x, y) => x * y,
    Vector::Three(x, y, z) => x * y * z,
};
println!("The vector has the magnitude '{}'", magnitude);
#
#}
```

(This gets even more exciting when we get into functions)

What happens if we add another variant to the enum though? Well, that `match` statement will see that not every case is
handled, and cause an error.

```rust
enum Vector {
    Two(usize, usize),
    Three(usize, usize, usize),
    Four(usize, usize, usize, usize),
}

fn main() {
    let v = Vector::Three(3, 4, 5);
    
    // This match will no longer compile
    let magnitude = match v {
        Vector::Two(x, y) => x * y,
        Vector::Three(x, y, z) => x * y * z,
    };
    println!("The vector has the magnitude '{}'", magnitude);
}
```

We can deal with this by either adding the missing case, or using `_`, which is a special variable that immediately 
discards whatever is put into it and will match anything.

```rust
#enum Vector {
#    Two(usize, usize),
#    Three(usize, usize, usize),
#    Four(usize, usize, usize, usize),
#}
#
#fn main() {
#    let v = Vector::Three(3, 4, 5);
#
let magnitude = match v {
    Vector::Two(x, y) => x * y,
    Vector::Three(x, y, z) => x * y * z,
    // This specific example isn't great, now any variant that doesn't match will return zero, an error might be better
    _ => 0,
};
#    println!("The vector has the magnitude '{}'", magnitude);
#}
```

Patterns on match arms are tested from top to bottom happen from top to bottom, and you can also match on more specific
patterns, like values:

```rust
#enum Vector {
#    Two(usize, usize),
#    Three(usize, usize, usize),
#}
#
#fn main() {
let v = Vector::Two(0, 0);

let magnitude = match v {
    // This arm will match, print the statement and return 0
    Vector::Two(0, y) =>  {
        println!("Hey, did you know that x was zero?");
        0
    },
    // Although `v` does match this arm, because we already matched on the previous arm, this block won't be run
    Vector::Two(x, 0) => {
        println!("Hey, did you know that y was zero?");
        0
    }
    // Nor will this one
    Vector::Two(x, y) => x * y,
    Vector::Three(x, y, z) => x * y * z,
};
#    println!("The vector has the magnitude '{}'", magnitude);
#}
```

There's one more trick up `match`'s sleeve which is match guards. Say we want to do something similar to the above, but
instead of matching on exactly zero, we want to match on values less than 10. We could make an arm for every variant, or
we could use a match guard which is like a mini if statement:

```rust
#enum Vector {
#    Two(usize, usize),
#    Three(usize, usize, usize),
#}
#
#fn main() {
let v = Vector::Two(0, 0);

let magnitude = match v {
    // This arm will match, print the statement and return 0
    Vector::Two(x, y) if x < 10 =>  {
        println!("Hey, did you know that x was small?");
        x * y
    },
    // Although `v` does match this arm, because we already matched on the previous arm, this block won't be run
    Vector::Two(x, y)  if y < 10  => {
        println!("Hey, did you know that y was small?");
        x * y
    }
    // Nor will this one
    Vector::Two(x, y) => x * y,
    Vector::Three(x, y, z) => x * y * z,
};
#    println!("The vector has the magnitude '{}'", magnitude);
#}
```

Looping
-------

### Loop

- no finish, only loop
- labels

### While

- exit condition in loop

### For In

- 