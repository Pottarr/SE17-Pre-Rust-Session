# Topics
    - Installing Rust
    - Windows
    - MacOS and Linux
    - Rust Compiler and Cargo
    - rustc (Rust compiler)
    - cargo (Rust packages manager)
    - rustc VS cargo
    - Basic Syntax
    - print! and println!
    - Commenting Code
    - Variables and Data Types
    - let Keyword
    - mut Keyword
    - Shadowing
    - Scalar Data Types
        - Integers
        - Floating-point Numbers
        - Boolean
        - Character
    - Operators
    - Operator Types (By number of operands)
        - Unary Operators
        - Binary Operators
    - Operator Types (By their use cases)
        - Arithmetic Operators
        - Comparison Operators
        - Logical Operators
        - Compound Assignment Operators
        - Parentheses
    - Control Flow Keywords
    - if-else statements
    - Nested if statements
    - match statements
    - Loops
    - Normal Loop
    - For Loop
    - While Loop





# Installing Rust

## For Windows
[Follow this link](https://www.rust-lang.org/tools/install) and
download from the 64-bit version of the installer.

> [!NOTE]
> May need to install Visual Studio

After downloading the **rustup-init.exe** from the link above, open
the executable (.exe) file, enter 1 into the terminal and submit.
This process will download the automnatically install rust via
Visual Studio and finish the installation.  

![Rust intallation terminal Windows](img/Rust_installation_terminal_Windows.png)


## For MacOS or Linux
```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

> [!IMPORTANT]
> Prerequisite: Need curl installed, you may ask TA for help.

### Understanding the command
Just to let you know, every time you get any bash command from the
internet, MAKE SURE you understand the context of it!  

This command is the download command using `curl` with flag `--proto`
to check for the link to be `https`, the flag `--tlsv1.2` is to force
to use `TLS version 1.2+` and the flag `-sSf` in the end is to
**silent when successfully installed and thow error when failed**.





# Rust Compiler and Cargo

Rust is a compiled programming language, therefore it needs a compiler.  

There are 2 ways to compile rust code.

## rustc (Rust Compiler)

`rustc` is mainly used in small project which has only one rust (.rs)
file and does not rely on dependencies.  

To use `rustc`,

```
rustc your_program_name.rs -o your executable_file_name
```

> [!TIP]
> If you realize the name of the file, you can see that Rust
> programming language has its own convention about naming file.
> It is recommended to use `snake_case` for most of the element of
> your codes or programs.

It is optional to declare the flag `-o` (output name flag). If you do not
declare it, the compiler will name the executable file after your
program name.  

> [!IMPORTANT]
> For Windows, you need to have `.exe` after the executable files, while
> in MacOS and Linux, it is unnecessary. (On Windows, after the `-o`
> flag, it is unnecessary to enter `.exe` afte the executable file name,
> since the compiler can figure it out by itself)  

## cargo (Rust packages manager)

`cargo` is mainly used in real-world project which has hierarchy of files
and rely on dependencies. You can add or remove dependencies in the
`cargo.toml` file. You can also create a documentation for your Rust
project using cargo too.  

To create a new Rust project using `cargo`,  

```
cargo new your_project_name
```

> As you can see, cargo prefer `snake_case` to be the naming format of
> your project name too.  

**DO NOT FORGET TO ENTER THE DIRECTORY BEFORE PROCEEDING OTHER COMMAND
VIA CARGO**  

```
cd your_project_name
```

To build the project as the execuatble file using `cargo`,

```
cargo build
```

To run the project code using `cargo`,

```
cargo run
```

To create the project's documentation using `cargo`,

```
cargo doc
```
To open the project's documentation using `cargo`,

```
cargo doc --open
```

## rustc VS cargo

| Use cases | rustc | cargo |
| --------- | ----- | ----- |
| Project Size | One file | Project with hierarchy of files |
| Dependencies | Does not support | Can manage using `cargo.toml` |
| Documenting | No built-in for that | Can create it using `cargo doc` |

# Basic Syntax

## print!() and println!()

In Rust, we use print!() and println!() **macros** to show the output
on the terminal.  

> [!IMPORTANT]
> Macros are not Functions but we will not dive into it in this 2 days
> session.  

Now, let's write our first program!

```rs
fn main() {
    println!("Hello, World!");
}
```

To explain what is going on, we need to know that Rust code will start
executing only when it has got the main function somewhere in the code.  

Unlike C and C++, Rust main function can be anywhere in the code.
It can above, under, or in the middle of other functions

> We will talk more about function in the following day session.  

## Commenting Code

In rust we use double forward slashes `//` to comment code.

For example,  

```rs
fn main() {
    println!("Hello, World!");
    // println!("This won't be printed hehe");
}
```

The output should be  

```
Hello, World!
```





# Variable and Data Types

## Variables

A variable is a representation of a value to make a certain value
reuseable and keep the code clean.  

There are two types of variables, mutable and immutable.  

Mutable variables means you can always change the value inside the
variable after declaring it. Immutable is the opposite, you cannot
change the value after it has been declared.

### let Keyword

To declare a variable, use the keyword `let`  

This is how you declare an immutable variable:

```rs
let x = 5;
```

### mut Keyword

Use the keyword `mut` to declare mutable variables.  

This is how you declare an mutable variable:

```rs
let mut x = 5;
```

If you try to change the value of an immutable variable, you will
receive an error:

```rs
fn main() {
    let x = 5;
    x = 6; // <--- This line will generate error.
    println!("x is: {}", x);
}
```

The output should be  

```
   Compiling playground v0.0.1 (/playground)
warning: value assigned to `x` is never read
 --> src/main.rs:2:9
  |
2 |     let x = 5;
  |         ^
  |
  = help: maybe it is overwritten before being read?
  = note: `#[warn(unused_assignments)]` on by default

error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:3:5
  |
2 |     let x = 5;
  |         - first assignment to `x`
3 |     x = 6; // <--- This line will generate error
  |     ^^^^^ cannot assign twice to immutable variable
  |
help: consider making this binding mutable
  |
2 |     let mut x = 5;
  |         +++

For more information about this error, try `rustc --explain E0384`.
warning: `playground` (bin "playground") generated 1 warning
error: could not compile `playground` (bin "playground") due to 1 previous error; 1 warning emitted
```

> [!NOTE]
> This code was run with `cargo run` in [Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024).
> A web-based Rust Programming Language code runner.  

## Shadowing

Shadowing is when you declare a new variable with the same name as
a previous variable. We can say that the first variable is shadowed
by the second variable

```rs
fn main() {
    let x = 5;

    {
        let x = "Pizza";
        println!("x is: {}", x); // <--- Pizza
    }

    println!("x is: {}", x); // <--- 5
}
```

The output should be  

```
x is: Pizza
x is: 5
```

We can also use shadowing to change a variable data type.

```rs
let x = "Pizza"; // <--- x is: Pizza
let x = x.len(); // <--- x is: 5
```

## Scalar Data Types

In today session, we are only going to talk about `Scalar Data Types`.

### Integers

We all know what are integers. There are two types of integers,
singed and unsigned.  

| Length | Signed | Unsigned |
| ------ | ------ | -------- |
| 8-bit | i8 | u8 |
| 16-bit | i16 | u16 |
| 32-bit | i32 | u32 |
| 64-bit | i64 | u64 |
| 128-bit | i128 | u128 |

Singed contains negative values and unsigned contains only positive
values. For example i8 will contain -128 to 127, u8 will contain
0 to 255.  

> [!NOTE]
> The default integer type of Rust Programming Language is `i32`  

### Floating-point Numbers

Rust only have f32 and f64, which are 32-bit and 64-bit in size respectively.

> [!NOTE]
> The default floating-point number type of Rust Programming Language is `f64`  

To declare variables with specific data types, you can do so like this:

```rs
let x: i32 = 5;
let y: f32 = 6.4;
```

This one shows that we declare the variable "x" with the data type i32 and
variable "y" with type f32.

### Boolean

Boolean types have two values, true and false.  

To declare boolean variables, use the `bool` keyword.

```rs
let is_this_true: bool = true;
```

Boolean values are often used for control statements like if/else.

### Character

`char` type is single character types.  

To declare, use `char` keyword and single quotes (''). The `char` type can
only be 1 character long.  

```rs
let meow: char = 'A';
```

# Operators

Like in mathematics, any expression has operators and operands.
You will see an example from the next diagram.  

```
                    This is an operator
                             |
                             ∨
                           7 + 2
                           ∧   ∧
                           |   |
                    These are operands
```

## Operator Types (By number of operands)

In this 2 days session we will talk about 2 types of operators.

- Unary Operators
- Binary Operators

### Unary Operators

The unary operators are operators which need only 1 operand.

```rs
let negative_int = -2; // This is a negative sign operator.
let wrong = !true; // This is a logical negation operator.
```

### Binary Operators

The binary operators are operator which need 2 operands. This types of
operators are the most commonly found in everyday life.

## Operator Types (By their use cases)

### Arithmetic Operators

In Rust, arithmetic operators are used to do the arithmetic
calculation as like in normal mathematics.

```rs
fn main() {
    let negative = -5;
    let addition = 3 + 1;
    let subtraction = 1 - 3;
    let multiplication = 4 * 2;
    let division = 5 / 2;
    let modulo = 14 % 3;
    println!("Negative: {}", negative);
    println!("Addition: {}", addition);
    println!("Subtraction: {}", subtraction);
    println!("Multiplication: {}", multiplication);
    println!("Division: {}", division);
    println!("Modulo: {}", modulo);
}
```

The output should be  

```
Negative: -5
Addition: 4
Subtraction: -2
Multiplication: 8
Division: 2
Modulo: 2
```

> [!IMPORTANT]
> If you look carefully at the Division, you will see that the result
has no decimal points. That is because the compiler assumes that the
number being operated are 32-bit integer (i32).

### Comparison Operators

In Rust, comparison operators are used to compare values or
expressions, and they return a boolean value (true or false).  

| Operator | Name | Description |
| -------- | ---- | ----------- |
| `==`     | Equality                 | Returns `true` if both values are equal.                                |
| `!=`     | Inequality               | Returns `true` if the values are not equal.                             |
| `<`      | Less than                | Returns `true` if the left value is less than the right.                |
| `<=`     | Less than or equal to    | Returns `true` if the left value is less than or equal to the right.    |
| `>`      | Greater than             | Returns `true` if the left value is greater than the right.             |
| `>=`     | Greater than or equal to | Returns `true` if the left value is greater than or equal to the right. |

```rs
fn main() {
    let a = 5;
    let b = 10;

    println!("a == b: {}", a == b);
    println!("a != b: {}", a != b);
    println!("a < b: {}", a < b);
    println!("a <= b: {}", a <= b);
    println!("a > b: {}", a > b);
    println!("a >= b: {}", a >= b);
}
```

The output should be  

```
a == b: false
a != b: true
a < b: true
a <= b: true
a > b: false
a >= b: false
```

### Logical Operators

In Rust, logical operators are used to combine or invert boolean
expressions. They're most commonly used in `if` statements and similar
control flow constructs.

| Operator | Name | Description |
| -------- | ---- | ----------- |
| `&&`     | Logical AND | Returns `true` only if **both** expressions are `true`. |
| `\|\|` | Logical OR | Returns `true` if **at least one** expression is `true`. |
| `!` | Logical NOT | Inverts the value: `true` becomes `false` and vice versa. |

```rs
fn main() {
    let a = true;
    let b = false;

    println!("a && b = {}", a && b);
    println!("a || b = {}", a || b);
    println!("!a = {}", !a);
}
```

The output should be  

```
a && b = false
a || b = true
!a = false
```

### Compound Assignment Operators

In most programming languages, we have compound assignment operators
to shorten the expressions.  

| Operator | Description | Equivalent To |
| -------- | ----------- | ------------- |
| `+=` | Addition assignment | `a = a + b` |
| `-=` | Subtraction assignment | `a = a - b`   |
| `*=` | Multiplication assignment | `a = a * b` |
| `/=` | Division assignment | `a = a / b` |
| `%=` | Remainder assignment | `a = a % b` |

For example,  

```rs
fn main() {
    let mut num: i32 = 11;
    println!("The value of the number is {}", num);
    num += 2;
    println!("The value of the number is {}", num);
    num -= 3;
    println!("The value of the number is {}", num);
    num *= 4;
    println!("The value of the number is {}", num);
    num /= 5;
    println!("The value of the number is {}", num);
    num %= 6;
    println!("The value of the number is {}", num);
}
```
The output should be  

```
The value of the number is 11
The value of the number is 13
The value of the number is 10
The value of the number is 40
The value of the number is 8
The value of the number is 2
```

### Parentheses

In Rust Programming Language, we use parentheses to control control
order of operations.

```rs
fn main() {
    let a = 2 + 3 * 4;       // = 14 (3 * 4 first, then + 2)
    let b = (2 + 3) * 4;     // = 20 (2 + 3 first, then * 4)
    println!("{}, {}", a, b);
}
```

The output should be  

```
14, 20
```





# Control Flow Keywords

## if-else Statements

### if Keyword

As previously mentioned in the Scalar Data Types Section, Boolean values are
used in if-else brackets. So in this section, we will be talking about
control flow statements (AKA. if else statements)  

An `if` expression allows you to execute some code depending on the conditions
given. The codes in the if block will execute if the given conditions are all
true. Otherwise it will skip the codes in the if block.  

For example:  

```rs
let x: i32 = 5;

if x < 10 {
    println!("{} is less than 10!", x);
}
```

The output should be  

```
5 is less than 10!
```

### else Keyword

But if we change x to, for example, 12, the above code will not print anything
since the condition is false. To handle the remaining condition cases, we add
an `else` bracket to out code.  

```rs
let x: i32 = 12;

if x < 10 {
    println!("{} is less than 10!", x);
}
else {
    println!("{} is more than or equal to 10!", x);
}
```

The output should be  

```
12 is more than or equal to 10!
```

### else if Keyword

To handle more than 2 conditions, we can use an `else if` block to handle other
possible conditions.  

If we change x to 10 and add another condition check:  

```rs
let x: i32 = 10;

if x < 10 {
    println!("{} is less than 10!", x);
}
else if x == 10 {
    println!("{} is equal to 10!", x);
}
else {
    println!("{} is more than 10!", x);
}
```

The output should be  

```
10 is equal to 10!
```

> [!TIP]
> If we use a boolean variable in a if else condition, we can omit the `==`
> comparision operator entirely.  

```rs
let true_var: bool = true;

if true_var
{
    println!("true");
}
```

The output should be  

```
true
```

### if-else in Variables Assignment

Another cool thing about if else statements is that it can also be used to
assign a value to a variable!  

For example:

```rs
let condition: bool = true;
let num: i32 = if condition { 5 } else { 6 };

println!("The number is {}", num);
```

The output should be  

```
The number is 5
```

Here we use if else to assign the value to the variable `num`, the `num`
variable will contain the number 5 since the condition is true.

## Match Keyword

The last control flow keyword we are going to talk in this session is
the `match` keyword.  

The `match` keyword is used for **pattern matching**, which allows you to
compare a value against a series of patterns and execute code based on
which pattern matches.  

```rs
fn main() {
    let key_pressed = 'a';
    match key_pressed {
        'w' => println!("Move Forward"),
        'a' => println!("Move Left"),
        's' => println!("Move Backward"),
        'd' => println!("Move Right"),
        _ => println!("Invalid Input"),
    }
}
```

The output should be  

```
Move Left
```





# Loops

Rust has 3 types of loops.

## Normal Loop
The normal will execute the code in ts scope repeatly until i meets
the condition and breaks inside the loop itself.  

For example, 

```rs
fn main() {
    let mut num: i32 = 10;
    loop {
        if num == 0 {
            println!("Happy New Year!!!");
            break;
        } else {
            print!("{} ", num);
            num -= 1;
        }
    }
}
```

The output should be  

```
10 9 8 7 6 5 4 3 2 1 Happy New Year!!!
```

## For Loop

This type of loop will loop through the iterator.

> [!NOTE]
> We will not cover the iterator in this 2 days session but it will be
> cover after midterm.  

For example,  

```rs
fn main() {
    for i in 1..3 {
        println!("Count is: {}", i);
    }
}
```

The output should be  

```
Count is: 1
Count is: 2
```

Wait there is no `Count is: 3`! Yes, the iterator in the for loop is
exclusive at the ending boundary.  

To make the iterator inclusive we should at `=`.  

For example,  

```rs
fn main() {
    for i in 1..=3 {
        println!("Count is: {}", i);
    }
}
```

The output should be  

```
Count is: 1
Count is: 2
Count is: 3
```

Another example which will be helpful in the lab,  

```rs
fn main() {
    for i in 1..=5 {
        for _ in 1..=i {
            print!("*");
        }
        println!();
    }
}
```

The output should be  

```
*
**
***
****
*****
```

Another example,  

```rs
fn main() {
    for i in (1..=5).rev() {
        for _ in 1..=i {
            print!("*");
        }
        println!();
    }
}
```

The output should be  

```
*****
****
***
**
*
```

As you see, the iterator can be reverse using `.rev()` method.

> [!NOTE]
> Well now, what in the world is METHOD? We will not cover in this
> 2 days session but it will be taught in the OOP concept, hopefully
> before midterm or later.  

## While Loop

In Rust we normally declare a mutable variable or more to initialize
the while loop.  

For example we can rewrite our old code as like this  

```rs
fn main() {
    let mut i: i32 = 1;
    let mut j: i32 = 1;

    while i <= 5 {
        while j <= i {
            print!("*");
            j += 1;
        }
        println!();
        i += 1;
        j = 1;
    }
}
```

The output should be  

```
*
**
***
****
*****
```

And also vice versa,  

```rs
fn main() {
    let mut i: i32 = 5;
    let mut j: i32 = 1;

    while i >= 1 {
        while j <= i {
            print!("*");
            j += 1;
        }
        println!();
        i -= 1;
        j = 1;
    }
}
```

The output should be  

```
*****
****
***
**
*
```

---

And that is all for today Rust Lecture! We hope to see you again
tomorrow (25/06/2025). There will be a lab session in the afternoon.
Feel free to ask question with TAs as you wish.

---
Lecturers:  
[Pottarr](https://github.com/Pottarr) (P'Potter)  
[Krakenlord5](https://github.com/Krakenlord5) (P'Tang)
