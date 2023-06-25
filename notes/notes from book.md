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