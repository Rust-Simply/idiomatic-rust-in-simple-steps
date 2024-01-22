Other Learning Resources
========================

Its worth mentioning that there are many other Rust learning resources out there, so if this guide ends up not working
out for you, or you're looking to supplement it with other sources, these are in my mind, the (other) best ways to learn
the Rust.

1. [The official book](#the-official-book)
2. [Rustlings](#rustlings)
3. [Rust by Example](#rust-by-example)
4. [Idiomatic Rust in Simple Steps](#idiomatic-rust-in-simple-steps)

The official book
-----------------

[The Rust Programming Language](https://doc.rust-lang.org/book/) book is the official way to learn, and will be the
most complete guide. This is how I learned the language way back in 2017 and its gotten significantly better since then.
The main difference between the official book and this guide is that the official book focuses on teaching you how the
language works, while this guide is focused on how the language is written, only looping back to how the language works
to explain why we write it that way. This guide tries to follow a similar path to the official book and to Rustlings,
see below, so it should be easy to jump between them.

Rustlings
---------

[Rustlings](https://github.com/rust-lang/rustlings/) is a "learn by doing" guide to Rust. It works by giving you
specific exercises to help you understand the language a bit at a time.

The gamification of the course makes it really fun, so I still use Rustlings to help keep myself sharp.

There is also an [IntelliJ Edu adaptation](https://plugins.jetbrains.com/plugin/16631-learn-rust/about) of Rustlings so
if you install the free Community version of IntelliJ IDEA, you can have an in editor tutorial.

Rust by Example
---------------

[Rust by Example](https://doc.rust-lang.org/stable/rust-by-example/) does exactly what it says on the tin. It focuses on
the implementation of the Rust language.

This is great to quickly look things up if you know what you want to know, however, there's some minor
pseudo-anti-patterns here that if you don't already know are anti-patterns, you might be tempted to copy-paste. Don't
worry though, we'll cover these here but also tools like Clippy (which we'll also cover) will be quick to point them
out.


Idiomatic Rust in Simple Steps
------------------------------

So, why use IRISS if there are these other great resources?

Idiomatic Rust in Simple Steps lines up nicely with these other resources and can supplement them or vice versa. While
the other resources focus on simply learning the language, IRISS focuses on how and why we write the language the way
we do. This might seem like the same thing, but here we'll ramp up on idioms quickly so that you can understand other
peoples code as well as write code that other people can understand.

This matters!

My biggest hot take of the software industry is that: Software engineers are not paid to write code, they're paid to
solve problems.

The best engineers I've worked with have solved problems without writing a single line of code, and the cheapest code to
maintain is the code you didn't write. Beyond this though, solving problems, even with code, takes understanding and
collaboration. If when you join a team you already understand why they write code the way they do, you're going to
become productive much faster.
