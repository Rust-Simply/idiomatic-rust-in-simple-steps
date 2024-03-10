Data Types
==========

Scalar Types
------------

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

This is how many scalar types there are in Rust! And yes, as scary as it is, you will completely understand this in
just a few minutes!

First and most importantly, forget the above, there's really only 4 subtypes that we actually care about:

- integers
- floating points
- characters
- booleans

We'll go over each of these individually, explain how they work, their variations and what you might use them for.

Before we do, lets very quickly cover binary though:

### Binary

Humans (mostly) count in base 10. That's numbers going from 0 to 9. You can imagine numbers as a series of columns,
where each column represents how many 1s, 10s, 100s, etc there are in the number.

For example, the number 123 contains one lot of 100, two lots of 10, and three lots of 1 

| Columns: | 100s | 10s | 1s |
|----------|------|-----|----|
| Numbers: | 1    | 2   | 3  |

So 9 goes to 10, 19 goes to 29, and 99 goes to 100 (because increme)

Computers can only really cope with things that are `on` or `off` though. How do you count with only on or off?

### Integers

### Floating Points

### Characters

### Booleans

Compound Types
--------------

### Arrays

### Tuples

### Structs

### Enums

Generic Types
-------------
