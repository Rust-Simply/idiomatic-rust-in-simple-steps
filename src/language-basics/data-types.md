Data Types
==========

Primitive Types
---------------

Scalar types are effectively the building blocks of all other types.

I think this is an early point in learning Rust that scares off a lot of potential new Rust engineers. You see, Rust has
a lot of scalar

I'm going to show
this to you, but I don't want you to worry about it. You, whoever you are dear reader, have already achieved things more
complicated than learning this

So, are you ready to see something terrifying that long before the end of the chapter you're going to have a complete
handle on?

| types             | 8 bit | 16 bit | 32 bit | 64 bit | 128 bit | memory width |
|-------------------|-------|--------|--------|--------|---------|--------------|
| unsigned integers | u8    | u16    | u32    | u64    | u128    | usize        |
| signed integers   | i8    | i16    | i32    | i64    | i128    | isize        |
| floating points   |       |        | f32    | f64    |         |              |
| characters        |       |        |        | char   |         |              |
| booleans          | bool  |        |        |        |         |              |
| string slices     |       |        |        |        |         | &str         |

This is how many scalar types there are in Rust! And yes, as scary as it is, you will completely understand this in
just a few minutes!

First and most importantly, forget the above, there's really only 4 subtypes that we actually care about:

- integers
- floating points
- characters
- booleans
- string slices

We'll go over each of these individually, explain how they work, their variations and what you might use them for.

Before we do, lets very quickly cover binary though:

### Binary Primer

Don't panic! No one is expecting you to learn to count in binary. Counting in binary is fun, but pretty useless. 😅

All I want to do is show you how things are represented in memory because it's going to make all those Rust types make a
lot of sense!

Humans (mostly) count in base 10. That's numbers going from 0 to 9. You can imagine numbers as a series of columns,
where each column represents how many 1s, 10s, 100s, etc there are in the number.

For example, the number 123 contains one lot of 100, two lots of 10, and three lots of 1

| Columns: | 100 | 10 | 1 |
|----------|-----|----|---|
| Numbers: | 1   | 2  | 3 |

When we add numbers to the columns, if the column goes over 9, then we add to the next column along.

So, if we add 1 to 9, it goes to 10, 19 goes to 29, and 99 goes to 100 (because the roll over from the right most 9 adds
to the next 9 also causing it to roll over).

This counting system is called base 10 by the way as each of those columns is 10 raised to the power of which column it
is, starting at 0:

- 10^0 = 1
- 10^1 = 10
- 10^2 = 100
- 10^3 = 1000
- etc

Electronics, and by extension computers, can only really cope reliably with things that are `on` or `off` though. How do
you count with only on or off? Well, what if instead of having ten possible values in each column (0-9 or base 10), we
only have two (0-1 or base 2). This is binary.

In binary our columns are a bit different:

- 2^0 = 1
- 2^1 = 2
- 2^2 = 4
- 2^3 = 8
- etc

So if we want to represent the number 11 in base 2, we can see it contains one 8, one 2, and one 1.

| Columns: | 8 | 4 | 2 | 1 |
|----------|---|---|---|---|
| Numbers: | 1 | 0 | 1 | 1 |

Sometimes when we want to write something in binary and be explicit that that is the system we're using we might write:
0b1011. This makes it clear that this number represents "eleven" and not "one thousand and eleven".

Each column is a ***b***inary dig***it***, which is where we get the term "bit".

Eight bits is a byte, and can represent the numbers from `0b0000_0000` (zero) to `0b1111_1111` (two hundred and fifty
five, again, I'm not expecting anyone to be able to _read_ this).

The reason why a byte is eight bits has a lot of history, but it pretty much comes down to character encoding (with 7
bits, you can represent 127 characters which covers lowercase, uppercase, numbers 0-9, various whitespace and
punctuation, and still have 1 bit left over for simple error checking).

> As a total aside, as a software engineer, you're very likely to also see number written in hexadecimal (base 16).
> This is because hexadecimal, is really nice when working with bytes. One byte (8 bits) perfectly maps to two
> hexadecimal digits. Hexadecimal digits go from 0 to 15, but are represented as 0-F (ie: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
> A, B, C, D, E, F).
>
> 0xF is 15, and so is 0b1111. The number 255 is much easier to write as 0xFF than 0b1111_1111

### Integers

Now that you've had that primer on binary, I bet those 12 different integer types are starting to make a lot more sense!

The most basic number type in Rust is the `u8`. This is an _unsigned_ integer (represented by the `u`) that is 8 bits in
length. Unsigned means that the number can only be positive (it does not have a negative sign). You might have already
guessed, but this is one byte, and can hold the numbers 0 to 255. A byte like this can be used for all sorts of things,
though you might commonly find it used for part of a color. We often represent colors as 8 bits of red, 8 bits of green,
8 bits of blue and sometimes 8 bits of transparency.

`i8` is an integer that can represent both positive and negative numbers (i.e. it's signed). It also only uses 8 bits
of data but in order to represent a number, however, instead of going from 0 to 255, it goes from -128 to 127.

You never need to know this, _but_, if you're interested in the mathematics of how it does this, it uses a method called
[two's complement](https://en.wikipedia.org/wiki/Two%27s_complement).

This, however, is complicated, and we don't think like computers. The easiest way to think about it is the left most
column is the negative version of itself, and all other numbers are the same. So, the number -125 can be represented as
0b1000_0011.

| Columns: | -128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|----------|------|----|----|----|---|---|---|---|
| Numbers: | 1    | 0  | 0  | 0  | 0 | 0 | 1 | 1 |

ie, the number contains one -128, one 2 and one 1, adding (-128 + 2 + 1) them is -125.

So that's `u8` and `i8`, and now you've probably guessed that for all the other integer types;

- `u` means it can only be positive
- `i` means it can be positive or negative
- the number after is how many bits are available to the number

Now we can build up a little table to show the minimum and maximum of these types:

| type |                                                  min |                                                 max |
|------|-----------------------------------------------------:|----------------------------------------------------:|
| u8   |                                                    0 |                                                 255 |
| i8   |                                                 -128 |                                                 127 |
| u16  |                                                    0 |                                              65,535 |
| i16  |                                               -32768 |                                              32,767 |
| u32  |                                                    0 |                                       4,294,967,295 |
| i32  |                                          -2147483648 |                                       2,147,483,647 |
| u64  |                                                    0 |                          18,446,744,073,709,551,615 |
| i64  |                           -9,223,372,036,854,775,808 |                                                 127 |
| u128 |                                                    0 | 340,282,366,920,938,463,463,374,607,431,768,211,455 |
| i128 | -170,141,183,460,469,231,731,687,303,715,884,105,728 | 170,141,183,460,469,231,731,687,303,715,884,105,727 |

Wow, those numbers get big fast!

There's still two types missing though; `usize` and `isize`.

In this case, the `size` is also acting as the number of bits, however, unlike the other integer types, the size of
`size` is variable.

Rust is a compiled language, meaning that the code you write in Rust is transformed into instructions that a CPU can
understand. CPUs are all different, but they typically follow what is called an "architecture". For example, if you're
reading this on a Windows or Linux desktop or an Intel Mac, the architecture of your CPU is _probably_ `x86_64`. If
you're reading this on an "Apple Silicon" Mac then the architecture is `arm64`.

> A quick aside, the world of CPU architecture is a bit of a mess so `x86_64` may also be referred to as `amd64` as AMD
> were the designers of the architecture, but it was designed to be backwards compatible with Intel's `x86`
> architecture. Similarly `arm64` is also sometimes referred to as `AArch64`.

When you compile Rust it will compile into an instruction set for the architecture your machine uses (though you can
also tell it what instruction set to compile for if you want to build it on one architecture but run it on another).

`x86_64` and `arm64` are both 64bit architectures, so when you build for these machines, the `size` in `usize` and
`isize` becomes `64`. However, if you were to compile for, say, a Cortex-M0 chip, then the instruction set would likely
be `Thumb-1` which is 16bit so the `size` in `usize` and `isize` becomes `16`.

#### Which integer is right for you?

You might think the obvious thing to do would be to use the largest possible number, for example, you can fit pretty
much every whole number you could possibly need into `i128`, so why use anything else?

There's two things to think about, first, what is the intended use of the number and, second, what is the architecture
of the machine you're running on?

Often times in software engineering, a number isn't just a number, it represents something. As we mentioned earlier,
colors are often (but not always), represented as 0 to 255 for each of red, green and blue. This means that a `u8` is
the best way to store these. If you combine those three colors with another 8 bits for transparency (alpha), then you
have four lots of `u8` which can be represented as a `u32`.

`u8` is also a good size for representing a stream of unicode characters, which is where we get `UTF-8`, the default
encoding for Rust strings.

For larger numbers though, you still may not want to use the largest. While you can use integers that are wider than the
architecture that you're running your program on, mathematics with those numbers will be slower. The CPU can only
process so many bits at once, so when it has numbers larger than that, it has to do multiple rounds of processing to
achieve the same results as it might have done if those numbers were stored in smaller integers.

By default, when you write an integer and store it in a variable, Rust will play it safe and use an `i32` as it doesn't
know what you might want to do with the number, and `i32` will fit inside most CPU architectures without needing extra
work.

```rust
let a = 10; // i32
```

However, it is more idiomatic to be intentional about the types you use. My methodology here is roughly:

- does this number represent something of a specific size like a color or ascii character, in which case, use that size
- is this number going to be used to access an array, in which case it really ought to be a `usize`
- am I more worried about the number slowing the program down than I am about accidentally trying to store a big number
  in a small integer, in which case `usize` or `isize`
- otherwise, if I'm ok potentially sacrificing speed, then an `i32` or `i64` is fine

You can specify what type a number is either by annotating the variable you are putting it inside:

```rust
let a: u64 = 10; // u64
```

Or, if that's not possible because you are, for example, passing the number to a function that could take many number
types, you can write the type at the end of a number:

```rust
some_generic_function(10u8); // u8
```

#### A brief note on Type Conversion

Finally, you can convert between types in several ways, which we'll talk about more later, but I wanted to quickly go
over some code from the last chapter.

In the bonus section of the last chapter, we got the number of milliseconds that had passed since midnight on the 1st
of January 1970, and then immediately used `as usize`:

```rust
let time = std::time::UNIX_EPOCH
    .elapsed()
    .expect("Call the doctor, time went backwards")
    .as_millis() as usize; // We only need the least significant bits so this is safe
```

The reason for this is the number of milliseconds since that date is approximately 1,710,000,000,000 and is
returned as a `u128`. We wanted to use this as part of a calculation to work out an index into an array. Indexes in
arrays are always `usize`. If you were to compile this program on a 32bit architecture, then the number of milliseconds
is greater than what would fit into a `usize` which would be a mere 4,294,967,295. When we use `as` it simply takes the
number, whatever it is and tries to cram it into the size `as <type>`.

When going from a larger size to a smaller size (in this case, from `u128` to the equivalent of `u32`) it simply cuts
off the front of the data, leaving the least significant bits. You can see this in the following program (don't forget
you can run this in place with the play button):

```rust
#fn main() {
let time = std::time::UNIX_EPOCH
    .elapsed()
    .expect("Call the doctor, time went backwards")
    .as_millis();

let time_u32 = time as u32;

println!("Before conversion: {time}");
println!("After conversion: {time_u32}");
#}
```

### Floating Points

We've covered twelve different ways of storing whole numbers in Rust, but there are only two ways of storing numbers
with decimal points: `f32` and `f64`.

Floating point numbers are things like `0.123` or `1.23` or even `123.0`. They're called floating point because the
decimal point can move around (as opposed to fixed point, where there is always the same number of fractional digits).

You're immediate thought here might be that you should use `f32` on 32bit systems, and `f64` on 64bit systems, but
actually this isn't the way to think about these numbers.

You see, floating points are not perfectly accurate. The bits of a floating point number are broken into parts:

- a sign (+/-)
- an exponent
- a fraction

Without going into too much detail on floating points this gives us a way of expressing very large numbers and very
small numbers but not every number in between (after all, there are infinite numbers between 0.0 and 1.0).

```rust
# fn main() {
    println!("520.02 - 520.04 should be -0.02");

// Single Precision Floating Point
    let float_32 = 520.02_f32 - 520.04_f32;
    println!("f32, {float_32}");

// Double Precision Floating Point
    let float_64 = 520.02_f64 - 520.04_f64;
    println!("f64, {float_64}");
    #
}
```

This is why floating point numbers should not be used for anything where accuracy matters, for example, anything to do
with money. Instead, if the currency you're representing uses "hundredths" for its minor currency like USD or GBP, then
you can represent the total number that instead, eg of cents for dollars or pennies for pounds.

When should you use floats?

Floating point numbers are great for more abstract mathematics where perfect precisions isn't strictly necessary, for
example, vectors, matrices and quaternions which are often used in applications like video games.

As to which you should use, you might think that it comes down to architecture again, for example, a program targeting
a 32bit architecture should use an `f32` and a 64bit architecture should prefer an `f64`... but if that's the case,
where is the `fsize`?

Actually, 32bit architectures are usually designed to support 64bit floating point numbers just fine, the difference
between `f32` and `f64` is that regardless of architecture, `f32` is faster, and `f64` is more "fine grain".

And thats all the challenging stuff done! Now for the easy bits

### char

`char` represents a single unicode character and is always 4 bytes (32bits) in size. In Rust a character is always
written between single quotes, where as string literals (as covered last chapter) are always written between double
quotes.

You can use any valid unicode character whether that's the upper or lowercase english letters A-Z, numbers 0-9, white
space characters, word characters from languages like Chinese and Japanese, emoji, or anything else that's a "unicode
scalar value".

```rust
# fn main() {
    let a = 'a';
    let five = '5';
    let yuki = '?';
    let happy = '😀';
    println!("{a} - {five} - {yuki} - {happy}");
    #
}
```

We usually use characters in relation to finding things inside strings. You can also turn strings into a collection of
characters and vice versa, however its important to note that a character inside a string may not take up 4 bytes (for
example, english letters and numbers only take 1 byte), however, once turned into a character, it will take up four
bytes.

### Boolean

There is only one booelan type in Rust: `bool`. It represents `true` or `false`.

> Useless (but interesting!) information: In terms of how much space it uses, Rust considers it to be a single bit (an
> i1) _however_ LLVM, which is a tool Rust uses as an intermediate compilation step, will use a full byte, though the
> value inside the byte will still be 0 for false and 1 for true.
>
> Weirdly, if Rust got its way, the decimal value for a boolean as its stored in memory would be 0 for false and -1 for
> true (remember in i numbers, the left most bit is its negative self). Not that it matters, its just interesting 😅

Boolean values are usually reserved for `if` statements, and this is a good thing to look out for as finding it else
where _might_ be a sign that the code isn't written in the best way.

### String slices

Our old friend the string slice! It is the only Rust primitive that isn't also a scalar value (it isn't of fixed size).

You will never actually see something of the type `str`, you will usually see this as a reference to a string slice
(`&str`).

Compound Types
--------------

### Arrays

### Tuples

### Structs

### Enums

Generic Types
-------------
