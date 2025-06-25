# Lab 2 (25/06/2025)

## How to INPUT Text?
Just to give you some hint about the upcoming labs, so here is the
way we get text input from the terminal in Rust.

```rs
use std::io;

fn main() {
    let mut input = String::new();
    println!("Please input your name: ");
    io::stdin().read_line(&mut input).expect("Failed to read.");
    let name = input.trim().to_string();
    input.clear();

    println!("Hello, {}!", name);
}
```


The output should be  

```
Please input your name: 
```

After entering your name, the result should be  

```
Please input your name: Pottarr
Hello, Pottarr!
```

That's all.  
If you want to input number as i32, you can use `.parse()` to
convert it.

For example, here is the real question that you should **ANY TYPE OF LOOP**
from the yesterday session.  

```rs
use std::io;

fn main () {
    let mut input = String::new();
    println!("Please enter the head of the multiplication table: ");
    io::stdin().read_line(&mut input).expect("Failed to read.");
    let head = input.trim().to_string().parse().expect("Invalid Number");

    // Your code here...
}
```

<!-- For first person who can spot the bug in this program, there'll be a prize for that! -->

The output should be  

```
Please enter the head of the multiplication table: 
```

After entering a number, the result should be  

```
Please enter the head of the multiplication table: 5
5 * 1 = 5
5 * 2 = 10
5 * 3 = 15
5 * 4 = 20
5 * 5 = 25
5 * 6 = 30
5 * 7 = 35
5 * 8 = 40
5 * 9 = 45
5 * 10 = 50
5 * 11 = 55
5 * 12 = 60
```

## Question 1 The grade checker

Write a grade analyser program for you to find whether your grades is
correct or not from these following informations.

- Your transcription

| Subject | Your Score (Max: 100) | Your Grade |
| ------- | --------------------- | ---------- |
| ComProg | 68 | C |
| EleSysProg | 79 | B+ |
| IntroCalc | 88 | A |
| IntroLogic | 73 | B |
| CircElect | 65 | C+ |

- The score system

| Score (MaxL 100) | Grade |
| ---------------- | ----- |
| A | 80 - 100 |
| B+ | 75 - 79 |
| B | 70 - 74 |
| C+ | 65 - 69 |
| C | 60 - 64 |
| D+ | 55 - 59 |
| D | 50 - 54 |
| F | 0 - 49 |

> [!TIP]
> Use for loop to iterate trhough the array of scores.  

Code:  

```rs
fn main() {
    let scores: [i32; 5] = [ 68, 82, 88, 73, 65];
    // [ Comprog, EleSysProg, IntroCalc, IntroLogic, CircElect ]

    for score in &scores {
        // Your code here...
    }
}
```

Compare the output with your transcription and list the subject with
incorrect grade in the lab sheet.  

## Question 2 Write a Rust Program that:

1. Prompts the user to enter two names: one for Player 1 and one for Player 2.
2. Prints the names in two different formatted patterns: one vertical and one horizontal, using loops to print the border of asterisks.

### Example:
If the user inputs the names "Mike" for Player 1 and "Leo" for Player 2, the output should be:

Vertical Pattern:
```
******************
*                *
* Player 1: Mike *
*                *
******************
*                *
* Player 1: Leo  *
*                *
******************
```


Horizontal Pattern:
```
**********************************
*                *               *
* Player 1: MIke * Player 2: Leo *
*                *               *
**********************************
```

### Hint:

Read string:

```rust
io::stdin().read_line(&mut playerl).expect("Fai1ed to read");
```

Length of string:

```rust
player.len()
```

## Question 3 Write a Rust program that simulates a simple employee data processing system:

1. Prompts the user to enter the salaries of 5 employees and stores them in an array.
2. Prompts the user to enter employee details as a tuple consisting of the employee's name age and salary.
3. Prints the all employee details in a formatted message. Name: <name> Age: <age> Salary: <salarp
4. Prints the total salary, average salary, employee with the highest salary and employee with oldest.

### Example:
Output should be:
```
Employee 1: Name = "Alice", Age = 30, Salary = 30000
Employee 2: Name = "Bob", Age = 41, Salary = 45000
Employee 3: Name = "Charlie", Age = 38, Salary = 50000
Employee 4: Name = "Diana", Age = 43, Salary = 35000
The employee with the highest salary is: Charlie with a salary of 50000
The oldest employee is: Diana, 43 years old
```
