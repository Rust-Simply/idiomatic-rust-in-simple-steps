# Guessing Game

We're going to start in the same place as the official Rust book,
[with a guessing game](https://doc.rust-lang.org/book/ch02-00-guessing-game-tutorial.html)! However, we're going to
do things a little bit differently.

We'll start as we did with Hello World, and create a new project with cargo

```shell
$ cargo new guessing-game
```

Opening `src/main.rs` we#ll see the same code as before:

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

And a quick description:

```rust
#fn main() {
#     println!("Welcome to the guessing game!");
    println!("I have chosen a number between 1 and 100, can you guess it?");
#}
```

Lets pick a number that the user has to guess, something between 1 and 100 would be nice, and I'm going to stick this
at the top of the function (don't forget you can click the &#x1f441; on the example to see)

```rust
#fn main() {
    let actual = 53;
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a number between 1 and 100, can you guess it?");
#}
```

In Rust, variables have a "type", that is to say, they have certain behaviors and representations depending on the type.
By default, a number is represented as an `i32`, that is a 32 bit signed integer. This means that it can represent a 
number between `-2,147,483,648` and `2,147,483,647`.

There are many other ways to represent numbers, from big to small, positive only or allowing negative numbers, allowing
decimal places or exactly sized to system memory. We'll talk more about how to specify the type and how to decide the
right number type for you next time, but for now an `i32` is just fine.

Lets output the number (even though we haven't asked for a guess yet)

```rust
#fn main() {
#     let actual = 53;
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a number between 1 and 100, can you guess it?");
    println!("The number I chose was {actual}");
#}
```

Great... not much of a game is it.

In order to get some user input, we need to read from the terminal. Before we tell the user what the actual number was
lets ask them to guess

```rust,noplayground
#fn main() {
#     let actual = 53;
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a number between 1 and 100, can you guess it?");
#    
    println!("Enter a number between 1 and 100");
#    
#     println!("The number I chose was {actual}");
#}
```

We're then going to read a line of input from the user. When the program runs in the terminal, we can ask the user to
type things, regardless of whether you are on Windows, Mac or Linux, this input is passed into the program through a
stream of data called `stdin` (standard in).

Rust comes with a "standard library" that we can access through `std`. I always say S T D, but you may also here people
call it "stud". Inside of this is a module (a grouping) called io that deals with input and output. If we weren't using
the `println!` macro, this is where we'd have to come to write things out to the terminal too, via a stream called 
`stdout` (standard out). 

> For completions sake I should mention there is one more stream called `stderr` (standard error). This is also an
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
> There are more streams, denoted by numbers, but these are rarely used and are way outside of the scope of this series.
>
> `stderr` is really useful for things like logging and we'll talk more about streams in the future.

So, we get stdin using `std::io::stdin()`, this is a function call that returns a temporary handle to the input stream.

The double colons just tell Rust that you're looking for something inside a module (that grouping I mentioned). We'll 
cover modules in detail later, including how, why and when to make your own, as well as better ways to access them, but
since we only need to write this line once, this is fine.

Off the back of this, we can call `.lines()` which is a method (a special type of function that belongs specifically to
some other thing, in this case the handle to that data). Below I put this on a new line for legibility, but you don't
need to do this.

`.lines()` returns an iterator, something we can step through a bit at a time. In this case, if the input was multiple
lines then the iterator would return one line at a time. We get the next line by calling `.next()` on the iterator.

If we add this all in our code looks like this

```rust,noplayground
#fn main() {
#     let actual = 53;
#     println!("Welcome to the guessing game!");
#     println!("I have chosen a number between 1 and 100, can you guess it?");
#    
#     println!("Enter a number between 1 and 100");
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

In software, functions can sometimes return something or nothing, and sometimes they could work or not work. These two
expects handle one case of each.

The first expect then:

```rust,ignore
.expect("No input read")
```

When we call `.next()` on any iterator, there either is something next or there isn't. In some languages this
might return either the data you expect, or a `null` value. This means you must either manually check the thing returned
was null, or don't check, and risk your program breaking at some other point when it expected that data but got null
instead. Obviously many people think this ambiguity is bad, including Tony Hoare, arguably the "inventor" of this kind
of behavior, who has called it his "billion-dollar mistake".

Rust does not allow you to use types like this interchangeably, the type can not be something or nothing, it must be
something that represents the concept of something or nothing, in this case a type called `Option`. Importantly the
`Option` itself is not the data you were expecting, so you are forced to check whether it contains that data before you
can use it. 

The idiomatic way to do this depends on whether your code can recover from the data not being there. In our case we're
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

    let guess: i32 = input.parse().expect("input was not a number");

    if guess == actual {
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

 
