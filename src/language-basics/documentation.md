Documentation
=============

Documentation. Is. Awesome... in Rust.

Documentation is awesome all the time but in Rust, it's truly magical. This is thanks to two tools: `rustdoc` and, 
spoilers, `cargo test`.

Libraries
---------

In order to show the value of documentation, we're going to start creating Rust libraries.

Up to now, we've been building binaries. Rust compiles binaries into executables, but those binaries can link to
libraries that provide functionality. This is particularly helpful when sharing and reusing code. We'll be talking much
more about how we share code with each other when we get to ecosystem, but even as we're just learning the language, we
can leverage the power of libraries for things like code reuse within the same project.

Let's try it out. First, lets make a normal program, remember you can do this with something like:
`cargo new iriss-documentation`

In `main.rs`, we'll make a single function and our main program will just check it works.

```rust
fn add(a: usize, b: usize) -> usize {
    a + b
}

fn main() {
    assert_eq!(add(40, 2), 42);
}
```

Next, lets migrate the add function to a library. Create a new file called `lib.rs`. In the same way that `main.rs` is
the entry to our binary, `lib.rs` is the entry point to our library. Our library acts like any other module and the
module is named the same thing as our project (though swap hyphens `-` for underscores `_`).

We'll move our add function to the `lib.rs` and make it public

```rust,no_playground
// lib.rs
pub fn add(a: usize, b: usize) -> usize {
    a + b
}
```

We can now access this function from our main via the project module (eg, `iriss_documentation`).

```rust,no_playground
# // This is included for `mdbook test` which I'm using to check for obvious mistakes,
# // this _should_ be in lib.rs in your project and not in a module
# mod iriss_documentation {
#     pub fn add(a: usize, b: usize) -> usize {
#         a + b
#     }
# }
// main.rs
use iriss_documentation::add;

fn main() {
    assert_eq!(add(40, 2), 42);
}
```

We can now share this code amongst multiple binaries. We _aren't_ going to do this, but we _could_ move `main.rs` to 
`src/bin` and then rename it. We can then have as many binaries as we like here, all using the same library. The reason
we aren't going to do this now is that if you have multiple binaries in the bin directory, you have to tell Rust which
one to run when you use `cargo run` with the `--bin` flag.

rustdoc
-------

We can create
