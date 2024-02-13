Setting Up
==========

Preamble
--------

So, you want to learn the Rust programming language but where do you even start?

- How do you install it
- How do you work with it
- How do you write our first program
- And whats going on inside

Today we're gonna answer these questions.

This series has a free accompanying book, in which you can find all the details for this tutorial, check the
description below.

My name is Daniel, welcome to IRISS

Installing Rust
---------------

How you install Rust depends on the operating system you're using.

I'm going to go over Mac and Linux first as they're both the same and quite simple.

Then I'll cover Windows which is a little bit different.

After that we'll go over setting up our development which is roughly the same for all systems, so use the video chapters
to skip to the parts of the video that are appropriate for you.

### Mac and Linux

Setting up Rust on Mac and Linux is exactly the same process... exactly the same, except for this one thing.

Before we begin, on Mac, you'll need to make sure you have xcode build tools. 

Do that by opening up a terminal window and typing xcode dash select dash dash install

My Mac already has xcode's tools installed, if you don't have it, but this takes a good 20 minutes, so be patient and
come back when its done.

On Linux, you'll need to make sure you have gcc. Different flavours of Linux use different package managers, but if
you're using Linux as your daily driver, I'm hoping you already know how to use yours.

To demonstrate on Raspberry Pi, I'm going to type sudo apt install gcc, though you can see this Pi already has it
installed.

Everything else on these two systems is the same, so, next, head over to rustup dot rs and pop this in your console.

You'll be asked if you want to customize the install, I'm not going to so I'll choose 1 and press enter.

After a minute or two, the installation completes and it'll suggests sourcing the cargo env file.

You only need to do this if you want to use Rusts tools in the current terminal session, which we do, so lets do that

Now we can check that everything has installed correctly by running cargo version

And you should see something like this

Thats it, we're ready to go. Head on to the Development Environment chapter unless you're particularly curious about
the Windows install

### Windows

#### Build Tools

Installing Rust on Windows is little bit more challenging but you've got this, I believe in you

First we're going to need Microsoft build tools. To get them we'll head to  visualstudio dot microsoft dot com slash
downloads and download the community edition

This installer is your gateway to a lot of stuff... almost none of which we actually need.

Click the tab for Individual components and search for the following

C++ x64 slash x86 build tools

Select the latest version

Next search for Windows SDK and choose the appropriate version for you.

I'm running Windows 11, so I'll choose the latest version of that SDK.

Then we'll click install and wait for it to do its thing. This can take a while so feel free to pause the video and
grab a cup of tea.

#### Rust up

Now that we've got the build tools we can get the rust installer which we get from rustup dot rs

Download rustup-init.exe

When we run this we're given the option to customize the installation. 

I'm not going to do that so I'll choose 1.

Once this is complete we can check that everything is working ok by opening a terminal (either cmd or powershell) and
running cargo version

And you should see something like this

#### GCC

Ok, there's one more step that is... arguably optional and we (probably) won't need it for this series but as soon as
you step outside the series, you'll run into problems without it

You see Rust is a compiled language that can talk to and use other bits of compiled code

There are different ways to compile code and although at this point, you _can_ compile anything we're going
to build in this series, you won't be able to compile everything that you could compile on Windows

For that we need GCC

The easiest way I've found to install GCC is via MSYS2

However, this... isn't great. If anyone knows of an easier way to get it, please please comment below.

In the mean time, lets do this

First, we're going to grab the installer from msys2 dot org 

and then run that

MSYS2 is a little bit fussy about where you install it so if in doubt just pop it where it wants to be

I'm going to install mine to the D drive in programs slash msys2

Wherever you put it, keep its location in mind, we'll have to find it again shortly

Once its installed, we need to make sure we have the ability to access any tools we install with it

We can do this by adding its binaries directory to the system path

This allows Windows to use anything installed there from anywhere else

First lets open the location where we installed it

Then open the ucrt64 directory, then bin

Copy the full address to the bin directory, you can do this by right clicking the address bar and clicking "Copy
Address"

Next we need to update the system path.

Go to your start menu and search for "environment", then click "Edit the system environment variables"

This, weirdly, doesn't take you straight to environment variables, but there is a button to do that at the bottom of
this screen.

Select Path, then hit edit

Click New on the right and paste in the address from earlier

Ok, last step

We're going to run the application MSYS2 UCRT64

And we'll run pacman dash capital S mingw dash w64 dash ucrt dash x86 underscore 64 dash gcc

You'll be asked for confirmation, just hit enter

This will install gcc and all of its dependencies

Once finished we can check this worked with gcc dash dash version

Phew, thats it, if you're still here, you are incredible!

The first time I tried this I honestly nearly cried

If you haven't done so already, its a good idea to restart your computer before moving on

Once you're back we'll set up a development environment

The Development Environment
---------------------------

A software developers environment is something very specific to them. Some people love vim and neovim, others like Nova,
I'm personally a big fan of the IntelliJ ecosystem.

If you've already found the tools for you and they have good Rust support, there's no wrong answer here, use whatever
makes you happy.

If you're new to software engineering though, and not looking to fork out potentially hundreds on tooling, I recommend
Visual Studio Code. 

It's free, it's perhaps the most widely used and supported editor today, and can be greatly extended through plugins.

It's the editor I'll be using throughout this series as I believe it will be the most familiar to people.

Lets go to code dot visualstudio dot com to download it

After installing it, we will need some plugins to work with Rust.

To get them, we'll start the app

Then click the extension button in the side bar

So... what do we need?

### rust-analyzer

First and perhaps most obviously, we need language support. Rust Analyzer will give us syntax highlighting, auto
complete, type hints, show documentation, allow symbol editing and will just generally make out lives a lot easier.

Lets grab that by clicking install

### CodeLLDB

This is the tool that's going to give us software engineering super powers

Sometimes you can be working on code and its not doing what you expect

"Happens to everyone all of the time, no need to worry"

Wouldn't it be cool to just step over each line of code as it runs and see exactly whats happening

LLDB will let us do that

We'll cover how it works in a bit, for now lets just grab it

### Even Better TOML

The Rust ecosystem makes heavy use of the TOML file format

We'll talk about this more later in the series but now is a good time to pick up this extension

### crates

Finally, Rust developers share code through a mechanism called Crates

The crates extension will help us keep any crates we're using up to date

Again, we're not going to dive into this just yet but we should get it now while we're here

Hello World
-----------

With all of that out of the way, we are _finally_ ready to create our first program

After all you've achieved already, you're not going to believe how easy the next bit is

The first program most developers write in a new language is called Hello World

All it does is that when you run it, it will, in some way, output Hello World, so lets do that

Lets open up a terminal and navigate somewhere we want to work

So, for example, on Mac or Linux, I might make a directory in my home folder

On Windows I might navigate to My Documents and make a directory there

Now we'll use cargo, remember cargo, from what seems like forever ago, to make our new project

cargo new hello hyphen world

Now we can open the project in visual studio code by typing code hello hyphen world

As you can see, cargo has created a few files for us

A .gitignore file which is useful for version control

Version Control Systems are out of scope of this series but I highly recommend looking into a system called Git

This will let you save your work in incremental steps as well as being an industry standard method of sharing and
collaboration

Cargo.toml describes the project as a whole, what dependencies it uses and any special preparations or settings it needs

We'll come back to this file in the future but for now we won't be using it

Finally we have our source directory in which all of our Rust code will live

The name of the file main.rs is used to tell Rust that we're going to make an executable program

Inside we'll see we can already see some code

Before I go through it all, lets run the program

For now we can do this from the terminal which we can access inside Code

Pro Tip for new Visual Studio Code users, if you want to do something and aren't sure how, you can hold Control (or 
Command on Macs) Shift and hit P

This brings up a little search bar you can use to find what you want in this case a terminal

Sometimes you might have to scroll a little to find the exact right thing but recently used things will filter to the
top so we don't have to do this too often

The terminal should open in our projects directory so all we have to do now is type cargo run

We get some output that its compiled and then we get our Hello World written to the screen!

Well that was easy. Our first rust program which we've arguably built ourselves without writing a single line of code
manually

You'll find that the Rust community has put a lot of effort into making Rust as easy as possible to learn an use

### Anatomy of Hello World

Looking at our main.rs lets break it down line by line

First we have this `fn main() {`

In Rust, fn is how we describe a function and a function is basically just a little chunk of code

We'll talk more about functions in a future video

The next bit "main" is the name of the funciton

We can usually call functions anything we like but when we create an executable program, one that can be run on its own,
we need a function called "main" to be the start point

The parentheses after the name of the function is where we normally put parameters

Parameters are how we pass data into a function, but we don't pass data into the main function so this is empty

The curly bracket that opens on this line has a matching closing curly bracket that denotes where the function ends,
everything inside is part of the function

Inside the function we have one line but lets break it into parts

First, the println (or print line) with an exclamation mark might look a bit like another function however, when you see
a label like print line followed by an exclamation mark its actually something called a macro.

Creating macros is a very advanced topic and we won't cover that until near the end of this series, however, they are
incredibly powerful and we will be using them a lot.

For now, don't worry about it, all you need to know is that they can do a lot more than functions and often don't
behave anything like functions

We still have parentheses delimiting the start and end of what we're passing to the macro, but in this case you _could_
also use square brackets or curly brackets, but for print line parentheses are the convention

Inside the print line we have a sequence of characters, surrounded by double quotes. This is, generally called a string.
Rust actually has lots of different kinds of strings, and we'll go into them more in the next video.

Feel free to change this to change the message being printed out.


Running and Debugging
---------------------

Before we end this video, I believe I promised you all some super powers

First lets make a quick change

Lets add our name into a variable.

I'll be talking more about variables in a couple of videos time, but quickly, we declare a variable with let, give it a
name put an equals sign after and then the value

String values like this need to be delimited with double quotes in Rust

Quite nicely this reads as let name equal Daniel

Another quick note: if your name does not use American English characters, this type of string in Rust is UTF-8 by
default and your name should be fine, however, your operating system might not be so when it writes it our, your milage
may vary. 

For example, my Mac is fine with my cats name, Yuki, but Windows, not so much

Anyway, we're going to modify our print line now to include the variable

There are a number of ways we can achieve this but the idiomatic way to do this is to use curly brackets and put the
variable name inside

Before we run our program again, if you mouse over the line number on line 3, you should see a little red dot appear,
click it

This is whats called a break point

Next we're going to run our program inside Code by pressing F5 and... uh oh... error message

Don't panic, this is just because we didn't tell Code how to run the program, luckily it can work it out for us

This varies a little bit on Mac and Windows but we're just going to tell it to run the program with LLDB

Now when we press F5 again, the program compiles and starts running but look! It stopped at the break point

We can now hover over the variable and see not only whats inside but how its stored

Having the ability to do this is incredible when trying to debug things or understand how something works

Next Time
---------

And that's it for today! It was a longer one today but honestly, just getting started might be the hardest part of the
whole series, but you did it and that's awesome, I'm legitimately proud of you

Next time we're going to make a slightly more advanced program by taking a bit of input and doing some comparisons

This way we can get a better feel for how Rust looks and feels.

If thats interesting to you, don't forget to like and subscribe

In the meantime, seriously, well done, take some time to reflect on your achievement, 

You're now an official Rustacean 

And I'll see you next time!
