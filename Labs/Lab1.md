# Lab 1 (24/06/2025)

## Question 1 Introduction to the wonderful world of SE

Write a "Hello, World!" program by yourself or copying this code.

Code:
```rs
fn main() {
    println!("Hello, World!");
}
```

Expected Output:
```sh
Hello, World!
```

> [!IMPORTANT]
> Use this command for compiling only in these 2 days labs.
```sh
rustc your_program_name.rs -o your_program_name
./your_program_name
```

## Question 2 print! Macro

PStringarse arguments into the `print!` macro ONLY to display these
informations.  

Code:
```rs
fn main() {
    let name = "Your Name here";
    let std_id = 68011234;
    let fav_color = "Your favorite color here";

    // Your code here...
}
```

Output:
```sh
Hello, my name is Your Name here (ID:68011234).
I like Your favorite color here color.
Nice to meet you!
```

> [!IMPORTANT]
> You can only use print! macro, println! macro is not allowed.

## Question 3 Multiplication Table

Build a Multiplication Table using for loop.  

Code:  

```rs
fn main() {
    let stop = 3;

    // Your code here...
}
```

Output:  

```
-- 1 --
1 * 1 = 1
1 * 2 = 2
1 * 3 = 3
1 * 4 = 4
1 * 5 = 5
1 * 6 = 6
1 * 7 = 7
1 * 8 = 8
1 * 9 = 9
1 * 10 = 10
1 * 11 = 11
1 * 12 = 12

-- 2 --
2 * 1 = 2
2 * 2 = 4
2 * 3 = 6
2 * 4 = 8
2 * 5 = 10
2 * 6 = 12
2 * 7 = 14
2 * 8 = 16
2 * 9 = 18
2 * 10 = 20
2 * 11 = 22
2 * 12 = 24

-- 3 --
3 * 1 = 3
3 * 2 = 6
3 * 3 = 9
3 * 4 = 12
3 * 5 = 15
3 * 6 = 18
3 * 7 = 21
3 * 8 = 24
3 * 9 = 27
3 * 10 = 30
3 * 11 = 33
3 * 12 = 36

...

-- 12 --
12 * 1 = 12
12 * 2 = 24
12 * 3 = 36
12 * 4 = 48
12 * 5 = 6o
12 * 6 = 72
12 * 7 = 84
12 * 8 = 96
12 * 9 = 108
12 * 10 = 120
12 * 11 = 132
12 * 12 = 144
```

## Question 4 Prime Agent

Build a program to detect prime numbers.

Code:  

```rs
fn main() {
    for i in 1..=20 {
        println!("{i} is a prime number?: {is_prime(i)}");
    }
}

fn is_prime(i: int) -> bool {
    // Your code here...
}
```

Output:  

```
1 is a prime number?: false
2 is a prime number?: true
3 is a prime number?: true
4 is a prime number?: false
5 is a prime number?: true
6 is a prime number?: false
7 is a prime number?: true
8 is a prime number?: false
9 is a prime number?: false
10 is a prime number?: false
11 is a prime number?: true
12 is a prime number?: false
13 is a prime number?: true
14 is a prime number?: false
15 is a prime number?: false
16 is a prime number?: false
17 is a prime number?: true
18 is a prime number?: false
19 is a prime number?: true
20 is a prime number?: false
```

## Question 5 Pyramid

Build a Pyramid using while loop  

Code:  

```rs
fn main() {
    let height = 4;

    // Your code here...
}
```

Output:  

```
   *
  ***
 *****
*******
```

## Question 6 Fibonacci

Write a program to display Fibonacci Numbers.  

Code:  

```rs
fn fib(x: i32) -> i32 {
    // Your code here...
}

fn main() {
    for i in 0..10 {
        println!("fib(i) = {}", fib(i));
    }
}
```

Output:  

```
fib(0) = 0
fib(1) = 1
fib(2) = 1
fib(3) = 2
fib(4) = 3
fib(5) = 5
fib(6) = 8
fib(7) = 13
fib(8) = 21
fib(9) = 34
```
