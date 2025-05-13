# Topics
- Installing Rust
- Rust Compiler and Cargo
- Basic Syntax
- Variables and Data Types
- Operators
- Control Flow Keywords
- Loops

# Installing Rust

## For Windows
[Follow this link](https://www.rust-lang.org/tools/install) and download from the 64-bit version of the installer.

> May need to install Visual Studio

After downloading the **rustup-init.exe** from the link above, open the executable (.exe) file, enter 1 into the terminal and submit. This process will download the automnatically install rust via Visual Studio and finish the installation. 

![Rust intallation terminal Windows](img/Rust_installation_terminal_Windows.png)


## For MacOS or Linux
```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

> Prerequisite: Need curl installed, you may ask TA for help.

### Understanding the command
Just to let you know, every time you get any bash command from the internet, MAKE SURE you understand the context of it!  

This command is the download command using **curl** with flag **--proto** to check for the link to be **https**, the flag **--tlsv1.2** is to force to use **TLS version 1.2+** and the flag **-sSf** in the end is to **silent when successfully installed and thow error when failed**.

# Rust Compiler and Cargo

Rust is a compiled programming language, therefore it needs a compiler.  

There are 2 ways to compile rust code.

## rustc (Rust Compiler)

**rustc** is mainly used in small project which has only one rust (.rs) file and does not rely on dependencies.  

To use **rustc**,

```sh
rustc your_program_name.rs -o your executable_file_name
```

> If you realize the name of the file, you can see that Rust programming language has its own convention about naming file. It is recommended to use **snake_case** for most of the element of your codes or programs.

It is optional to declare the flag **-o** (output name flag). If you do not declare it, the compiler will name the executable file after your program name.  

> For Windows, you need to have **.exe** after the executable files, while in MacOS and Linux, it is unnecessary. (On Windows, after the **-o** flag, it is unnecessary to enter **.exe** afte the executable file name, since the compiler can figure it out by itself)

## cargo (Rust packages manager)

**cargo** is mainly used in real-world project which has hierarchy of files and rely on dependencies. You can add or remove dependencies in the **cargo.toml** file. You can also create a documentation for your Rust project using cargo too.

To create a new Rust project using **cargo**,

```sh
cargo new your_project_name
```

> As you can see, cargo prefer **snake_case** to be the naming format of your project name too.

**DO NOT FORGET TO ENTER THE DIRECTORY BEFORE PROCEEDING OTHER COMMAND VIA CARGO**

```sh
cd your_project_name
```

To build the project as the execuatble file using **cargo**,

```sh
cargo build
```

To run the project code using **cargo**,

```sh
cargo run
```

To create the project's documentation using **cargo**,

```sh
cargo doc
```
To open the project's documentation using **cargo**,

```sh
cargo doc --open
```

## rustc VS cargo

| Use cases | rustc | cargo |
| ---------- | ----------- | ---------- |
| Project Size | One file | Project with hierarchy of files |
| Dependencies | Does not support | Can manage using **cargo.toml** |
| Documenting | No built-in for that | Can create it using **cargo doc** |

# Basic Syntax

## print!() and println!()

In Rust, we use print!() and println!() **macros** to show the output on the terminal.

> Important notice: Macros are not Functions but we will not dive into it in this 2 days session.

Now, let's write our first program!

```rs
fn main() {
    println!("Hello, World!");
}
```

To explain what is going on, we need to know that Rust code will start executing only when it has got the main function somewhere in the code.  

Unlike C and C++, Rust main function can be anywhere in the code. It can above, under, or in the middle of other functions

> We will talk more about function in the following day session.

## Commenting Code

In rust we use double forward slashes **//** to comment code.

For example,  

```rs
fn main() {
    println!("Hello, World!");
    // println!("This won't be printed hehe");
}
```

The output should be  

```sh
Hello, World!
```

# Variable Types

# Operators

# Control Flow Keywords

# Loops

Rust has 3 types of loops.

## Normal Loop
The normal will execute the code in ts scope repeatly until i meets the condition and breaks inside the loop itself.  

For example, 

```rs
fn main() {
    let mut num: i32 = 10;
    loop {
        if num == 0 {
            println!("Happy New Year!!!");
        } else {
            print!("{} ", num);
            num -= 1;
        }
    }
}
```


<!-- NOTE: Don't forget to mention the compound assignment operators at "num -= 1;" -->



The output should be  

```sh
10 9 8 7 6 5 4 3 2 1 Happy New Year!!!
```

## For Loop

This type of loop will loop through the iterator.

> We will not cover the iterator in this 2 days session but it will be cover after midterm.  

For example,  

```rs
fn main() {
    for i in 1..3 {
        println!("Count is: {}", i);
    }
}
```

The output should be  

```sh
Count is: 1
Count is: 2
```

Wait there is no "Count is: 3"! Yes, the iterator in the for loop is exclusive at the end.  

To make the iterator inclusive we should at "=".  

For example,  

```rs
fn main() {
    for i in 1..=3 {
        println!("Count is: {}", i);
    }
}
```

The output should be  

```sh
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

```sh
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

```sh
*****
****
***
**
*
```

As you see, the iterator can be reverse using **.rev()** method.

> Well now, what in the world is METHOD? We will not cover in this 2 days session but it will be taught in the OOP concept, hopefully before midterm or later.  

## While Loop

In Rust we normally declare a mutable variable or more to initialize the while loop.  

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

```sh
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

```sh
*****
****
***
**
*
```

---
Lecturers:  
[Pottarr](https://github.com/Pottarr) (P'Potter)  
[Krakenlord5](https://github.com/Krakenlord5) (P'Tang)