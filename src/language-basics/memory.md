# Memory

When you create a program, how does it remember things? In the last chapter, we created a variable and put our name
inside it. Where was our name stored?

We're going to make a simplified version of the 
[guessing game](https://doc.rust-lang.org/book/ch02-00-guessing-game-tutorial.html) from the official Rust book to
explore this. Lets start by making a new project by running this in your terminal

```shell
$ cargo new guessing-game
```

Opening `src/main.rs` we'll see the same code as before:

```rust
fn main() {
    println!("Hello, world!");
}
```

As we step through this tutorial, if you mouse over the code examples, you can see some buttons that will let you
1. &#x2398; Copy the example to your clipboard
2. &#x23f5; Run the example via [rust playground](https://play.rust-lang.org) (where possible)
3. &#x1f441; Show any code I've opted to hide for clarity, try this on the next block

Lets quickly change our hello world message to something that welcomes us to the game.

```rust
#fn main() {
    println!("Welcome to the guessing game!");
#}
```

And a ask the user to do something:

```rust
#fn main() {
#     println!("Welcome to the guessing game!");
    println!("I have chosen a color red, green or blue, can you guess which?");
#}
```

> Because we've used a second `println!` this will appear on a new line. The new line actually comes at the end of the
> `println!` so if you want to make both sentences appear on the same line, you can change the first one with `print!` 
> (no "ln"). Try it out and see what else you might need to change!

Lets pick a color that the user has to guess. To begin with we'll just hard code one value, later we'll make it choose
randomly. I'm going to choose blue, but you can choose whatever you like:

```rust
#fn main() {
    let actual = "blue";
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a color red, green or blue, can you guess which?");
#}
```

Lets output the color. This will end up being the last thing in the program but we can use this to check everything is
working ok:

```rust
#fn main() {
#     let actual = "blue";
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a color red, green or blue?");
    println!("The color I chose was {actual}");
#}
```

We can run this now and see that the color to be guessed was entered.

Great... but not much of a game is it.

In order to get some user input, we need to read from the terminal. Before we tell the user what the actual number was
lets ask them to guess

```rust,noplayground
#fn main() {
#     let actual = "blue";
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a color red, green or blue");
#    
    println!("Enter a number a color red, green or blue");
#    
#     println!("The number I chose was {actual}");
#}
```

We're then going to read a line of input from the user. When the program runs in the terminal, we can ask the user to
type things, regardless of whether you are on Windows, Mac or Linux, this input is passed into the program through a
stream of data called `stdin` (standard in).

Rust comes with a "standard library" (the name is unrelated to the stream) that we can access as a module called `std`.
I pronounce this S T D, but you may also here people call it "stud". Modules in Rust are a way of grouping up other bits
of code such as functions, data types and even other modules. We'll talk about them more in a future lesson. Inside of
this is another module called `io` that deals with input and output. If we weren't using the `println!` macro, this is
where we'd have to come to write things out to the terminal too, via a stream called `stdout` (standard out).

> For completion's sake I should mention there is one more stream called `stderr` (standard error). This is also an
> output stream that we can use to separate "good" output that is relevant to the normal use of the program to really 
> any other kind of output, whether that be errors or just information not directly relevant to the main output.
>
> For example, on Mac and Linux, if you use `cargo run 2> /dev/null` to run your program, you'll see that you lose the
> messages about your program being compiled because we redirected stderr (stream 2) to the void of `/dev/null`, and
> Cargo sensibly decided that _it's_ output is not relevant to your programs normal output
>
> On Windows the same can be achieved in cmd using `cargo run 2> nul` (note, only one l in nul), or in powershell with
> `cargo run 2> $null` (two l's this time and a dollar, no idea why its different)
> 
> There are more streams, denoted by numbers, but these are rarely used and are way outside the scope of this series.
>
> `stderr` is really useful for things like logging and we'll talk more about streams in the future.

So, we get stdin using `std::io::stdin()`, this is a function call (we'll talk about functions in a couple of chapters), 
that returns something called a "handle" that we can use for temporary access to the input stream.

The double colons just tell Rust that you're looking for something inside a module. We'll cover modules in detail later,
including how, why and when to make your own, as well as better ways to access them, but since we only need to write
this line once, this is the easiest way to do it.

We could store the result of `stdin()` in a variable, however, we only use this once and then we're done with it, so,
off the back of the function call, we can call immediately call `.lines()`. This is a method (a special type of function
that belongs specifically to some other thing, in this case it belongs to the handle for stdin). In the example below
I've put this on a new line for legibility, but you don't _need_ to do this.

`.lines()` returns an iterator, allowing us to iterate (step through) each line one at a time. We get the next line by
calling `.next()` on the iterator.

If we add this all in our code looks like this

```rust,noplayground
#fn main() {
#     let actual = "blue";
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a color red, green or blue");
#    
#     println!("Enter a red, green or blue");
    let input = std::io::stdin()
            .lines()
            .next()
            .expect("No input read")
            .expect("Could not get input from stdin");
    println!("Your guess was {input}");
#    
#     println!("The number I chose was {actual}");
#}
```

Wait wait wait, what are those `expect`s about?!

`expect()` is, I would say, the second worst way you could handle something going wrong in your program. You absolutely
should not use this in anything except the most throw away software as it will immediately cause the program to stop
and throw a lot of information at the user. In the future we'll talk about things going wrong and how to better handle
them, however, as this program is just for you, I think you'll cope for now. 😊

That doesn't explain what these lines are doing, or why there are two of them though. The reason for this is that there
are two possible ways `.lines()` might not work.

The first expect then:

```rust,ignore
.expect("No input read")
```

When we call `.next()` on any iterator, there either is something next or there isn't.  In some languages this
might return either the data you expect, or a `null` value. For example it might return `"red"` or `null`. "red" is a
string but null is not, what happens if you pass this to a function that expects a string? This means you must either
manually check the thing returned was null, or don't check, and risk your program breaking at some other point.
Obviously many people think this ambiguity is bad, including Tony Hoare, arguably the "inventor" of this  behavior, who
has called it his "billion-dollar mistake".

Rust does not allow you to use types like this interchangeably, i.e. data can not be a string or null as these types are
not compatible. In Rust we use a kind of container type to get around this called `Option`. Importantly, when a function
returns an `Option` type you, the programmer, must check to see if it contains something, and then extract the thing if
its there. There are a number of ways to do this and `.expect` is one of the worst ways to do this (we'll talk about
better ways in the future), as it will attempt to extract the data if its there, or stop the program 

For the time being we're
going to just say we can't deal with it not being there, we don't want to continue running the program and we want the
program to stop. We use `.expect("message")` to say, if this is nothing, we are giving up, stop the program and print
our message (plus a few other useful bits) to the `stderr` (see above).

This still isn't very idiomatic, ideally we'd convert the bad behavior into a proper error and handle it more sensibly
but for now its passable. Maybe... we'll fix this later. :)

Speaking of proper errors:

```rust,ignore
.expect("Could not get input from stdin");
```

If the Option we got from `.next()` contains something instead of nothing, it _still_ doesn't actually the users input.
It turns out that reading data from `stdin` is itself fallible.

Fallibility in programming is another thing we've traditionally handled very badly. A common way to deal with this in
other languages is to stop executing and "throw" an error. The big problem with this is function you are calling does
not explicitly tell you that its fallible (may fail). Worse, it may be that the person who wrote that function didn't
even know because the fallibility is deeply nested.

Rust does away with this with another type called `Result`. If your function can fail, it must return a Result type.
Like with `Option`, `Result` is its own type that contains either the data we wanted, or an error.

Again, the idiomatic way to handle this depends on what you're trying to do, is the error recoverable, and how do we
want to report the error back to the user. Using `expect` will, again, cause the program to immediately stop with the
error message and other bits sent to `stderr`.

But anyway, we now have a working program, there is an actual number, the user guesses a number, we print both to the
screen.

Going Further
-------------

The input data we get from the user is a string, not a number so, if I enter a string, it just prints that as my guess.

```text
Welcome to the guessing game!
I have chosen a number between 1 and 100, can you guess it?
Enter a number between 1 and 100
obvious nonsense
Your guess was obvious nonsense
The number I chose was 53
```

Because `actual` is an "i32" and `input` is a "String", we can't simply compare them, we need to convert one type into
the other. We can do this with a method on strings called `.parse()`. As you can probably guess `parse` is fallible and
if we give it `"obvious nonsense"` it will error. We can deal with that with another `expect`.

There's something else we need to do though, there are lots of things we could parse our string into, so we need to tell
Rust what we want. There's a couple of ways to do this, but the most idiomatic way, and what we'll do here, is just tell
Rust what the type of the variable is going to be, then Rust is smart enough to `parse` our String into that data.

Eg:

```rust,ignore
let guess: i32 = input.parse().expect("input was not a number");
```

That's pretty clean, right?

Now that the types are the same, we can do different things depending on whether the the numbers are a match. We can
do this with an `if` statement. You use an `if` by giving it something that will evaluate to either true or false. For
example `guess == actual` will be true if guess is exactly equal to actual, otherwise it will be false. We can follow
the `if` part immediately with an `else` that says what to do if the evaluation came to false.

Below is the full function using this if/else:

```rust,noplayground
fn main() {
    let actual = 53;

    println!("Welcome to the guessing game!");
    println!("I have chosen a number between 1 and 100, can you guess it?");

    println!("Enter a number between 1 and 100");
    let input = std::io::stdin()
        .lines()
        .next()
        .expect("No input read")
        .expect("Could not get input from stdin");

    if input == actual {
        println!("Correct!")
    } else {
        println!("That was not correct, try again")
    }
}
```

Ok, not bad. This is a bit more game like. I'm not happy about that last expect though. Its quite likely that the user
will enter something thats not a number, even if its just to "try it out". If you try it in your program, you'll see
rather a lot of unnecessary output:

```text
Welcome to the guessing game!
I have chosen a number between 1 and 100, can you guess it?
Enter a number between 1 and 100
obvious nonsense
thread 'main' panicked at src\main.rs:44:36:
input was not a number: ParseIntError { kind: InvalidDigit }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```


