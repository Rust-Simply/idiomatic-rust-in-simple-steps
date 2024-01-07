Why or Why Not, Installation and Hello World
============================================

1. [Why should you consider Rust](#why-should-you-consider-rust)
2. [Why not Rust](#why-not-rust)
3. [How to install Rust](#how-to-install-rust)
    1. [Mac and Linux](#mac-and-linux)
    2. [Windows](#windows)
4. [The Development Environment](#the-development-environment)
5. [Hello World](#hello-world)


Why should you consider Rust
----------------------------

I could tell you that its a "blazingly fast" language, that holds your hand and helps reduce bugs ,that its first party
tooling is second to none, that it has a mature ecosystem and an amazing community... but you're going to learn all
about that during this course. There is, in my mind, a singular reason everyone should learn Rust:

**Because its fun**

Seriously, Rust is my happy place. Sure it has its frustrations like any language, but when I work with Rust I _feel_
smart, I have less horrible surprises, and I have a lot of confidence that my code will "just work", that I won't need
to come back to fix bugs and if I do, I will be able to quickly understand old code, write new tests and fix any
problems.

But, I'm not alone, since its 1.0 release Rust has been Stack Overflows "most loved" (now "most admired") language eight
years in a row. This isn't derived from people like me just saying "its great" but is, of those who used the language
his year, what percentage still want to use it next year.

Why not Rust
------------

As much as I love Rust, there is a reason it may not be the language for you... and its a big one.

If you are specifically looking to learn a language to get a job (and you're not interested in Blockchain) Rust is not
going to be a good language... for now. The irony of "professional Rust" is that Rust engineers think there are no Rust
employers, and Rust employers think there are no Rust engineers _because_ very few of us are interested in Blockchain
technologies.

That said, things have been slowly changing. More businesses are picking it up due to its low cost to write, run and
maintain. The main cost to adopting Rust remains the cost of training people, but there are more and more of us out
there and perhaps 2024 will finally be the tipping point. 

None the less, right now, if you're looking for a job,
[better languages to learn](https://survey.stackoverflow.co/2023/#most-popular-technologies-language-prof) would be
[TypeScript](https://www.typescriptlang.org) and [Python](https://www.python.org).

How to install Rust
-------------------

### Mac and Linux

Go to [https://rustup.rs](https://rustup.rs), copy the command displayed there into your terminal.

// TODO: Get pictures, step through guide

Once the install is complete you can check whether everything has installed properly like this:

```sh
cargo version
```

You should see something like

```
cargo 1.75.0 (1d8b05cdd 2023-11-20)
```

Cargo is the main tool we'll use to build and work with Rust, you will need at least version 1.75 for this tutorial.
+

### Windows

Running Rust on Windows is a bit more challenging, but you've got this, I believe in you.

The first thing we're going to need are some Microsoft build tools. 

Head to [https://visualstudio.microsoft.com/downloads/](https://visualstudio.microsoft.com/downloads/) and download the
community tool.

The installer is the gateway to a huge number of tools and software, but we only need two things.

Click through to the Individual Components tab, and search for the following: `C++ x64/x86 build tools`

Select the latest version.

Next search for: `Windows 11 SDK`

Again, select the latest version


The Development Environment
---------------------------

Hello World
-----------