Source: https://doc.rust-lang.org/book/

# Chapter 1

No highlights/notes. This chapter involved installing Rust and learning a few build/run commands with Cargo.

# Chapter 2: Programming a Guessing Game

While reading about handling return values...

>As mentioned earlier, `read_line` puts whatever the user enters into the string we pass to it, but it also returns a `Result` value. [`Result`](moz-extension://f82c32a8-1cef-4470-bc5e-d82e081d0277/_generated_background_page.html../std/result/enum.Result.html) is an [_enumeration_](moz-extension://f82c32a8-1cef-4470-bc5e-d82e081d0277/_generated_background_page.htmlch06-00-enums.html), often called an _enum_, which is a type that can be in one of multiple possible states. We call each possible state a _variant_.
>
>`Result`’s variants are `Ok` and `Err`. The `Ok` variant indicates the operation was successful, and inside `Ok` is the successfully generated value. The `Err` variant means the operation failed, and `Err` contains information about how or why the operation failed.
>
>Values of the `Result` type, like values of any type, have methods defined on them. An instance of `Result` has an [`expect` method](moz-extension://f82c32a8-1cef-4470-bc5e-d82e081d0277/_generated_background_page.html../std/result/enum.Result.html#method.expect) that you can call. If this instance of `Result` is an `Err` value, `expect` will cause the program to crash and display the message that you passed as an argument to `expect`. If the `read_line` method returns an `Err`, it would likely be the result of an error coming from the underlying operating system. If this instance of `Result` is an `Ok` value, `expect` will take the return value that `Ok` is holding and return just that value to you so you can use it. In this case, that value is the number of bytes in the user’s input.

While reading about handling errors. You can use `parse()` which returns a `Result` type. `Result` types have two variants: `ok` and `Err`. We can use a `match` expression on the `Result` variant to handle errors.

>If `parse` is _not_ able to turn the string into a number, it will return an `Err` value that contains more information about the error. The `Err` value does not match the `Ok(num)` pattern in the first `match` arm, but it does match the `Err(_)` pattern in the second arm. The underscore, `_`, is a catchall value; in this example, we’re saying we want to match all `Err` values, no matter what information they have inside them.

Example error handling using `match`:
```rust
let guess: u32 = match guess.trim().parse() {
	Ok(num) => num,
	Err(_) => continue,
```

Above, the `Err(_)` represents all possible error return values.

# Chapter 3
I read this chapter outside of Obsidian. Here are the sections and highlights I made while reading it:

## Variables and Mutability
### Shadowing
>Shadowing is different from marking a variable as `mut` because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword. By using `let`, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed.

## Data Types
### Scalar Types
>A _scalar_ type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters.

### Integer Types
***unsigned integer*** is an integer that does not require a positive or negative sign; effectively it's always a positive number.

### Integer Overflow
>Rust performs two's complement wrapping. In short, values greater than the maximum value the type can hold "wrap around" to the minimum value that type can hold. In the case of `u8`, the value 256 becomes 0, the value 257 becomes 1, and so on. The program won't panic but the variable will have a value that probably isn't what you were expecting it to have.

### Character Type
>Note that we specifiy `char` literals with single quotes, as opposed to string literals, which use double quotes.

### Tuple Type
> A _tuple_ is a general way of grouping together a number of values with a variety of types into one compound type. Tuples have a fixed length: once declared, they cannot grow or shrink in size.

>We can also access a tuple element directly by using a period (`.`) followed by the index of the value we want to access.

### Array Type
>Arrays are useful when you want your data allocated on the stack rather than the heap (we will discuss the stack and the heap more in [Chapter 4](moz-extension://f82c32a8-1cef-4470-bc5e-d82e081d0277/_generated_background_page.htmlch04-01-what-is-ownership.html#the-stack-and-the-heap)) or when you want to ensure you always have a fixed number of elements. An array isn’t as flexible as the vector type, though. A _vector_ is a similar collection type provided by the standard library that _is_ allowed to grow or shrink in size. If you’re unsure whether to use an array or a vector, chances are you should use a vector. [Chapter 8](moz-extension://f82c32a8-1cef-4470-bc5e-d82e081d0277/_generated_background_page.htmlch08-01-vectors.html) discusses vectors in more detail.

## Functions

>Rust code uses _snake case_ as the conventional style for function and variable names, in which all letters are lowercase and underscores separate words.

>Rust doesn’t care where you define your functions, only that they’re defined somewhere in a scope that can be seen by the caller.

### Parameters
>In function signatures, you _must_ declare the type of each parameter. This is a deliberate decision in Rust’s design: requiring type annotations in function definitions means the compiler almost never needs you to use them elsewhere in the code to figure out what type you mean.

### Statements and Expressions
>Rust is an expression-based language, this is an important distinction to understand. Other languages don’t have the same distinctions, so let’s look at what statements and expressions are and how their differences affect the bodies of functions.
>
>-   **Statements** are instructions that perform some action and do not return a value.
>  -   **Expressions** evaluate to a resultant value. Let’s look at some examples.

>Statements do not return values.

>Expressions evaluate to a value.

>Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, and it will then not return a value.

## Control Flow

### Evaluating `if` critera
>Blocks of code associated with the conditions in `if` expressions are sometimes called *arms*.

>[...] results from each arm of the `if` must be the same type

>Rust will not automatically try to convert non-Boolean types to a Boolean. You must be explicit and always provide `if` with a Boolean as its condition.

### Loops within loops

>If you have loops within loops, `break` and `continue` apply to the innermost loop at that point. You can optionally specify a _loop label_ on a loop that you can then use with `break` or `continue` to specify that those keywords apply to the labeled loop instead of the innermost loop. Loop labels must begin with a single quote.

### Conditional loops with `while`

>When the condition ceases to be `true`, the program calls `break`, stopping the loop. It’s possible to implement behavior like this using a combination of `loop`, `if`, `else`, and `break`; you could try that now in a program, if you’d like. However, this pattern is so common that Rust has a built-in language construct for it, called a `while` loop.

# Chapter 4: Understanding Ownership

## What is Ownership?

### Heap vs Stack
Book talks about the difference between these two types of memory. In summary, stack is a super fast portion of memory that's only accessible via a stack interface. A stack interface is last in, first out similar to a paper inbox. Heap, on the otherhand, is a portion of memory that needs to be allocated to an object before it can be used. This portion of memory is access by interfacing with the object (eg: an array) or a pointer (eg: a object that is pointing to the location of said array). Heap is slower than Stack because (1) you need to seek to the position in memory to access it and (2) object updates may trigger reallocations (eg: array increases from initial size) and garbage collection (eg: releasing allocations where objects have been removed).

### Ownership rules
- Each value must have an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value is dropped.

### String vs String Literal
A String is a collection of characters. A String is an object that is stored in the heap and can be mutable. A String Literal, on the other hand, is a static collection of characters that never change. String Literals can be stored in the stack as they have a fixed size.

You can create a String object from a String Literal object by:
```rust
let s = String::from("hello");
```

The double colon operator (`::`) allows us to use the `from` function within the `String` type.

### Referencing Strings
```rust
s1 = String::from("something");
s2 = s1
```

In the above code, s2 becomes a String object like s1. That is, it copies the pointer location of the value, the length of the object, and the capacity of the object. The actual contents (eg: "something") is not copied to s2 meaning both s1 and s2 would be pointing to the same location. Find in other languages but not Rust. Rust is attempting to protect from "double free" errors when you release memory for s1 and s2 (and accidentally drop something else if s2 is dropped well after s1).

After the above code is run, Rust won't allow you to reference s1 anymore. Instead, Rust's approach is that s1 has been "moved" to s2.

The function `clone` is available when you want to copy both the object and its contents to a new variable. There's a potential for `clone` to be expensive.

### Referencing scalar data types
Unlike with String in the previous section, scalar data types are stored in stack with a fixed size. Referencing them in a subsequent variable assignment results in a copy: both variables are valid.

## References and Borrowing

### Reference
Like a pointer but is guaranteed to point to a valid valid of a particular type for the life of the references. Allows us to use values without having to update ownership every time we interface with them. When the scope of a reference ends, it does not drop the value as it doesn't have ownership of the value.

#### Example without references
```rust
fn main() { 
	let s1 = String::from("hello"); 
	let (s2, len) = calculate_length(s1); 
	println!("The length of '{}' is {}.", s2, len); 
	} 
	
fn calculate_length(s: String) -> (String, usize) { 
	let length = s.len(); 
	}
```

#### Example with references
```rust
fn main(){
	let s1 = String::from("hello");
	let len = calculate_length(&s1);
	println!("Then length of '{}' is {}.", s1, len)
	}

fn calculate_length(s: &String) -> usize {
	s.len()
	}
```

#### Differences in examples
- Ampersands represent references in both variable creation (eg s1) and function signature (eg s).

### Borrowing
The action of creating a reference is referred to as "borrowing". By default, references are immutable.

You can create references that are mutable by:
- Update variable creation to include `mut` to be mutable.
- Update function signature to accept a mutable reference.

Mutable references have a restriction: you can only create one mutable reference to an object at a time in a given scope. Back to back mutable references to the same object will cause compile errors. This mechanism helps avoid race conditions where processes are trying to update the same object at the same time. Alternatively, you can make multiple mutable references across many scopes so long as each scope only makes a single mutable reference.

### Scope
References scope starts where it in introduced and continues until the last time it is used. It is acceptable if a new reference is created after an old reference is used for the last time.

### Slices
A special type of reference that accesses a contiguous sequence of elements within a collection rather than the entire collection. A slice does not transfer ownership.

Example with string slices:

```rust
let s = String::from("hello world");

let hello = &s[0..5] // ref to chars 0 thru 5
let hello2 = &s[..5] // same as above when starting at 0

let world = &s[6..11] // ref to chars 6 thru 11
let world = &s[6..] // same as above when ending at len

let helloworld = &s[..] // ref to entire value of s
```

In the syntax `[iS..iE]` iS is index start and is inclusive of the value. iE is the index end  is exclusive so, so iE-1 is the final value in the slice.

String slice is signified by `&str`.

# Chapter 5: Structs

## Defining and Instantiating

Object that can hold multiple named data types as values.  Example of a user struct:

```rust
struct User{
	active: bool,
	username: String,
	email: String,
	sign_in_count: u64,
}
```

After defining it, we can instantiate an instance by assigning values:

```rust
fn main() {
	let user1 = User {
		active: True,
		username: String::from("someusername123"),
		email: String::from("someone@example.com"),
		sign_in_count: 1,
	}
}
```

Mutability is all or nothing for a struct in Rust. Either the entire struct can be changed or none of the struct can be changed.

You can access items within a struct using dot notation (eg: `user1.email`). If mutable, values can be reset by making an assignment.

### Field Init Shorthand
When using a function to instantiate a struct, you can use `field init shorthand` to assign input parameters directly to the struct without typing them twice (once in the function signature and again in the assignment). That looks like:

```rust
fn main(){
	user1 = build_user("email@", "name")
}

fn build_user(email: String, username: String) -> User {
	User {
		active: True,
		username, // field init shorthand
		email, //field init shortname
		sign_in_count: 1,
	}
}
```

### Struct Update Syntax
Allows you to create a new struct by referring a previous struct and providing any updates. Imagine we carried over `user1` from above and used it to create `user2`:

```rust
let user2 = User {
	email: String::from("another@example.com"),
	..user1
}
```

The `..user1` reference must come last. Any field defined above this struct update syntax will be created with whatever provided value. Any field not named will inherent a value from the referenced struct.

### Derived Traits
Traits are methods that can be implemented by types. Traits are similar to interfaces in other languages but with added capabilities. Traits are explained in later in the book but a quick intro is good now. That's because as we're building structs, we may want to debug them and understand their contents. To do that, it would be helpful if we could quickly print the struct to screen.

Structs can inherent traits by adding outer attributes ahead of defining said struct. In this case, we can add the `Debug` trait to a struct that will help print it to standard output. In Rust, this is referred to as deriving a trait. We can print the debug trait to screen by referencing `:?` within the curly brackets of a `println!` macro:

```rust
[derive(Debug)]
struct Rectangle {
	width: u32,
	height: u32,
}

fn main() {
	let rect1 = Rectangle {
		width: 30,
		height: 50,
	};

	println!("rect 1 is {:?}");
}
```

Prints...

```bash
$ cargo run
   Compiling rectangles v0.1.0 (file:///projects/rectangles)
    Finished dev [unoptimized + debuginfo] target(s) in 0.48s
     Running `target/debug/rectangles`
rect1 is Rectangle { width: 30, height: 50 }

```

### Debugging within structs
We can wrap an expression within a struct with a `dbg!` macro. When you run the program, you'll see a "debug print lint" that shows you the expression and it's result.

For example, if the width field in Rectangle was written as `width: dbg!(30 * scale` then we'd see something like the following print to screen:

```bash
$ cargo run
   Compiling rectangles v0.1.0 (file:///projects/rectangles)
    Finished dev [unoptimized + debuginfo] target(s) in 0.61s
     Running `target/debug/rectangles`
[src/main.rs:10] 30 * scale = 60
[src/main.rs:14] &rect1 = Rectangle {
    width: 60,
    height: 50,
}
```

## Struct method syntax
Some terminology...
- **function**: independent block of code that takes inputs (arguments), performs computation, and optionally returns a value. Can be defined at the module level or within other functions.
- **method**: function associated with a particular type (struct, enum, trait) and is called on instance of said type using dot notation. They're defined within the context of a type and have access to the data and behavior of their instance.

To create a method within a struct, use an implementation block (eg: `impl Rectangle`) as the space to write code. The first parameter of this block will be `&self` so it methods can borrow its own data. Methods can take ownership or make mutable references, too. In the case of calculating area from width and height values, borrow makes more sense because we just want to read the values and do a computation. No need to take ownership or alter either field.

Methods can have the same names as fields. When we follow dot nation with parentheses, Rust interprets this to call a method rather than retrieve a field. This pattern is also used to create 'getters' of field values when you want  public access to certain fields and private access to others.

## Associated Functions
Functions with an `impl` block are called associated functions as they're associated with the type named after `impl`. `Self` is not a requirement that don't need an instance of the type to work with. These are not methods but are commonly used as constructors.  Here's how that looks for defining an associated function named `square` with a one dimension parameter and returning a `Rectangle` type:

```rust
impl Rectangle {
	fn square(size: u32) -> Self {
		Self{
			width: size,
			height: size,
		}
	}
}
```

To call this function, we'd used the double colon operator:
```rust
let sq = Rectangle::square(3);
```

# Chapter 6: Enums and Pattern Matching

## Enums
Enums (name derived from enumerate) give you a means of saying a value is one of a possible set of values. Useful for when something can only one and only one value (eg: type of shape, color shade, side of dice).

Defining an enum:
```rust
enum IpAddrKind {
	V4,
	V6,
}

let four = IpAddrKind::V4;

let six = IpAddrKind::V6;
```

Instead of building a struct, you can attach data directly to an enum:
```rust
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

Enums can have any kind of type, including types you define yourself.

```rust
struct Ipv4Addr {}

struct Ipv6Addr {}

enum IpAddr {
	V4(Ipv4Addr),
	V6(Ipv6Addr),
}
```

Enums can also use implementation blocks to create methods.

Enums provide the ability to distinguish if an object has a value (Some) or not (None). Rust does not have a null; it uses this Some/None approach instead. It makes use of `Object<T>` - a unique type that tells Rust a given object can be Some or None. How you handle an event where an `Object<T>` responds differently between Some or None is handled with a match expression.

## `match` control flow

`match` is a control flow construct that executes a branch (or arm) of code when the first successful criteria test is met.

You can use curly brackets when you want a match expression to run multiple lines of code. You can also extract data from an enum and use it in the expression (eg: state design of a quarter).

```rust
fn value_in_cents(coin: Coin) -> u8 {
	match coin {
		Coin::Penny => {
			println!("Lucky penny!");
			1
		}
		Coin::Nickel => 5,
		Coin::Dime => 10,
		Coin::Quarter(state) => {
			println!("State quarter from {:?}", state);
			25
		}
	}
}
```

Matches are exhaustive; you must cover all possible values if you're matching against an enum or `Option<T>`. This is a useful protection when using `Option<T>` as it forces you to consider logic for when `None` is supplied as a value. Catch-all pattern can be used to consider patterns not declared in the match and still be considered exhaustive.

Within a match, another catch-all exists for when you don't want to bind the value of the match to anything. For example, `_` can be used to exhaust a match list and perform logic without regard to the supplied value. If you don't want to do anything at all, you can use an empty tuple (`()`) as the expression.

## concise control flow with `if let`

`if let` takes a pattern and an expression separated by an equal sign. If the pattern is true, then the expression is executed similar to a match arm. `if let` is not exhaustive so you end up writing less code if you only want to fire on certain patterns/values. You can also use an else statement to execute when no patterns yield true.

```rust
let config_max = Some(3u8);
if let Some(max) = config_max {
	println!("The maximum is configured to be {}", max);
} else {
	println!("some other message");
}
```
# Chapters 7 thru 11

> I noticed I was doing a lot of copy/paste while studying on a screen. I decided to switch over to studying in a physical book instead. There are highlights and notes captured in the physical book.

# Chapter 12 

> Sample application that required screen time.

## Standard output (stdout) vs standard error (stderr)

There are two types of output to the terminal:
- standard output (stdout): general information
- standard error (stderr): error messages

Pushing output to two streams allows users to choose to direct successful output (stdout) to a file while still print errors (stderr) to screen.

In rust, we can use `eprintln!()` in place of `println!()` to have errors pushed to stderr.


# Unique Terms

> TODO come back here and define the terms

- prelude: 