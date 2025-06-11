# Topics
- Basic Ownership
    - Rules of Ownership
    - Reference
    - Mutable Reference VS Immutable Reference
    - Dangling Reference
- Function
    - Arguments VS Parameters
    - Return Value
    - Recursive Function
- Complex Data Types
    - Sequence Data Types
        - Array
            - Declaration
            - Accessing Element
            - Multidimensional Array
            - Array Iteration
                - Iteration by Element
                - Iteration by Index
        - Vector
            - Declaration
            - Accessing
            - Multidimensional Vector
            - Vector Iteration
                - Iteration by Element
                - Iteration by Index
    - Tuple
        - Declaration
        - Accessing Element
    - Slice
        - Declaration
    - &str and String
    - Struct
    - Enum
        - Declaration
        - Standard Library Enum
            - Option
            - Result

# Ownership

One of Rust's most unique features is "Ownership". It enables Rust to
have memory safety without needing a garbage collector.  

Ownership in Rust is a set of rules that the program has to follow in
order to manage memory.  

## Rules of Ownership

There are 3 rules in Rust's Ownership.

1. Each value in Rust has an owner.
2. There can only be 1 owner at a time.
3. When the owner goes out of scope, the value will be dropped.

First, let's look at what scope is:  

```rs
fn main() {

    {
        let x = 5; // First we delare variable 'x' inside a scope.
        // Then we can use 'x' inside this scope.
    }

    // After the above scope, we can no longer use 'x' since it's already out of scope.
}
```

A scope is the range within the program in which the item is valid.  
For this example we declare the variable `x` inside a scope.
The variable can be used inside the scope but not after the scope is
done, since the variable is considered **out of scope**  

If we declare a variable before declaring a scope, we can also use
that variable inside the scope too.

```rs
fn main() {
    let x = 5; // We declare variable 'x'.

    {
        let y = 6; // We declare variable 'y'.

        // In this scope, both 'x' and 'y' can be used.
    }

    // After exiting the scope, only 'x' can be used.
}
```

Now let's look at moving datas.  
Take this example:  

```rs
let x = 5;
let y = x;
```

From this example, we can guess that we declare variable `x` with
value 5, then we declare variable `y` with the value of `x`, so that
means the value from `x` is copied to `y`. And we'd be right becuase
that's what's actually happening.  

Now take another example:

```rs
let s1 = String::from("hello");
let s2 = s1;
```

You would think that it's the same thing as the previous example, but
it's actually not. That's because the String type is not a simple data
type like i32, so it doesn't automatically copy the value. What it does
is copy the pointer to the value "hello" in memory, now we have
2 pointers pointing to the same value.  
Rust doesn't allow this because if both `s1` and `s2` goes out of
scope, it will try to free up the same memory, which will create
problems.  
So that's why after the line `let s2 = s1;`, Rust considered `s1` not
valid anymore and the value is moved to 's2' instead.  

> [!NOTE]
> We will talk about String later on in today session but here is the
> most understandable example.

If we try this:  

```rs
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

We will get an error since `s1` is no longer valid.  

If we want to actually copy the value to `s2` we can call the
`.clone()` function.  

```rs
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```

Here are more examples I take from the official website for more
understanding.

```rs
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // because i32 implements the Copy trait,
                                    // x does NOT move into the function,
    println!("{}", x);              // so it's okay to use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
    // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{some_string}");
} // Here, some_string goes out of scope and `drop` is called. The backing
    // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{some_integer}");
} // Here, some_integer goes out of scope. Nothing special happens.
```

But what if we want to write a function that doesn't want to take
ownership?  

> [!NOTE]
> Here's how we really use tuples in Rust. We will talk about tuple
> again later in today session.  

Take this example:  

```rs
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{s2}' is {len}.");
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len();

    (s, length)
}
```

## Reference

The code above earlier is an example on how you can still use a String
after a function, but this looks like a lot of work, imagine if we
have many Strings we have to deal with.  
This is where our next concept comes in. Instead of moving the value
into a function, we can `reference` them to not lose ownership.
A reference is like a pointer to a value.  

Here's a modified code:  

```rs
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

### Mutable Reference VS Immutable Reference

Using the `&`, we can refer to the value but not own it. So we can
still use `s1` after the function call. But what if we want to make
changes to our variable in a function?  

We can use what's called a `mutable reference` in our function.  

Here's an example:  

```rs
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Here we specify in the function parameter to be `&mut String` meaning
we can modify the value of `some_string` and the value of `s` will also
be changed.  
There are some restrictions to this, that is if you already have
a mutable reference of some value, you cannot have another mutable
reference of that same value. That means **there can only be 1 mutable
reference at a time**.  

For example:  

```rs
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;

println!("{}, {}", r1, r2);
```

This code above will generate an error, since we have 2 mutable
reference to the same variable, which is not allowed in Rust.  

But you can generate many immutable reference to the same variable.  

```rs
let mut s = String::from("hello");

let r1 = &s;
let r2 = &s;

println!("{}, {}", r1, r2);
```

This code is valid.  

Here are more examples to give more understanding.  

```rs
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
let r3 = &mut s; // BIG PROBLEM

println!("{}, {}, and {}", r1, r2, r3);
```

We can't have a mutable reference while we have an immutable
reference.  

We can fix this by using the variable.  

```rs
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
println!("{r1} and {r2}");
// Variables r1 and r2 will not be used after this point.

let r3 = &mut s; // no problem
println!("{r3}");
```

Here we use `r1` and `r2` in a function. The reference scope only
lasted until the last time it is used, and since we used it in
a function, the reference scope only lasted up to that function, and
then we can create a mutable reference without any problem.  

This one below is similar to the last example, the reference goes out
of scope, so we can create a new one.  

```rs
let mut s = String::from("hello");

{
    let r1 = &mut s;
} // r1 goes out of scope here, so we can make a new reference with no problems.

let r2 = &mut s;
```


With reference like these, you can accidentally create what's called
a `dangling reference`, which is basically a reference of nothing.  

For example:  

```rs
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
}
```

## Dangling Reference

Here we have a function that returns a reference to a String.
We create a String variable, and return a reference of the variable we
just created.  
Now when this function ends, the variable `s` will be dropped since
it's out of scope, which means the referece we returned is literally
pointing to nothing.  

So to fix this, we simply return the String instead.  

```rs
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```

# Function

In this section, we will be talking functions!  
Functions are an important part in programming as it makes your code 
more reuseable and have a cleaner look.  

In Rust, you write the main codes in the `main()` function.

For example, this below code writes a new function called
`another_function()` and the `main()` function calls that function.

```rs
fn main() {
    println!("Hello, world!");
    another_function(); // <-- This is how to call a function.
}

fn another_function() {
    println!("Another function.");
}
```

The output should be  

```
Hello, world!
Another function.
```

In Rust, the functions can be wrote above or below the main function.
Rust doesn't care about the placement of functions.  

## Arguments VS Parameters

We can define functions to accept `parameters`. We provide the variable
name and its type, similar to a variable declaration. If we have
multiple parameters, seperate them with commas.

```rs
fn main() {
    another_function(5, 12);
}

fn another_function(x: i32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

The output should be  

```
The value of x is: 5
The value of y is: 12
```

When we pass values into the function parameters, we call them
`arguments`, something like "We pass arguments into the function."  

## Return Value

Some functions can also have return values, these are useful when you
want to return a value into a variable or use in control flow
statements.  

We simply draw an arrow (->) and a type you want to return after the
function.  

For example:  

```rs
fn five() -> i32 {
    5
}

fn main() {
    let x = five();
    println!("The value of x is: {}", x);
}
```

The output should be  

```
The value of x is: 5
```

In Rust, you don't have to specify return with the word `return`, you
can simply type in a value without semicolon.

Another example:

```rs
fn main() {
    let x = plus_one(5);
    println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

The output should be  

```
The value of x is: 6
```

The function above accepts an argumet and returns itself plus 1.

## Recursive Function

Functions can also be called recursively, these are called
`Recursive Functions`  

For example:

```rs
fn factorial(x: i32) -> i32 {
    if x == 0 {
        1
    }

    x * factorial(x-1)
}

fn main() {
    let x: i32 = factorial(5);

    println!("The value of x is: {}", x);
}
```

The output should be  

```
The value of x is: 120
```

The above function computes factorial with a recursive method,
basically it keeps returning the arguments passed in times the
function with the argument decrement by 1 until the argument is 0, it
will only return 1, stops the recursion, multiply the result and
return it in the end.  

Unless the argument is 0 at first, the programming will simply return
1 and the output should be  

```
The value of x is: 1
```

# Complex Data Types

In Rust, the word `Complex Data Types` is mot officially defined but it
is used in this session to simply group these 3 types of data groups.

- Compound Types
    - Array
    - Tuple
    - Slice
- Custom Types
    - Struct
    - Enum
    - Constant
- Standard Library Types
    - String
    - Vector
    - Option
    - Result

> [!NOTE]
> We won't include `Slice` and `Constant` in this 2 days session.

## Sequence Data Types

In this topic, we'll talk about some more Data Types, mainly
Sequence Data Types.  

### Array

An array is a collection of data of the same type. Arrays are fixed
size, meaning new data cannot be inserted or old data can't be removed,
but you can change the value of the existing datas.  

#### Declaration

To create an array, we can create them inside a set of square brackets
`[]`, and to specify the type for variables, we can do it in the format
of `[T; N]` where `T` is the `type` and `N` is the `size` of our array.  

For example:  

```rs
let arr: [i32; 5] = [1, 2, 3, 4, 5];
```

Here we create an array of i32 with the array size of 5. Inside the
array we have the data 1, 2, 3, 4, 5.  

To create an array with the same data, we can use the `[T; N]` format.

```rs
let arr: [i32; 5] = [0; 5];
```

Here we create an array of zeroes, so now the array will have 5 zeroes
in it `[0, 0, 0, 0, 0]`.  

#### Accessing Element

To access array elements, we type in the name of our array followed by
a square bracket with an index number.  
Rust indexing starts at **0**. So if we want to access the first element,
we can do that with `arr[0]`.  

For example:  

```rs
let arr: [i32; 5] = [1, 2, 3, 4, 5];

println!("First element is {}", arr[0]);
println!("Second element is {}", arr[1]);
println!("Third element is {}", arr[2]);
```

The output should be  

```
First element is 1
Second element is 2
Third element is 3
```

To get the length of an array, we can use the `.len()` method.

> [!NOTE]
> We won't cover this in this session but you will learn about `.len()`
> method more in Struct implementation (`impl`) hopefully before
> midterm.  

```rs
let arr: [i32; 5] = [1, 2, 3, 4, 5];

println!("The length of this array is {}", arr.len());
```

The output should be  

```
The length of this array is 5
```

#### Multidimensional Array

We can also fit an array inside an array to make a 
`Multidimensional Array`.  

```rs
let arr: [[i32; 3]; 2] = [
    [1, 2, 3],
    [3, 2, 1]
];
```

Here we create an array with a size of 2. Inside we have an array with
a size of 3.  

To access the innermost element, we can use the double indexing like
`arr[0][0]`.

```rs
let arr: [[i32; 3]; 2] = [
    [1, 2, 3],
    [3, 2, 1]
];

println!("The first element of the first array is {}", arr[0][0]);
```

The output should be  

```
The first element of the first array is 1
```

#### Array Iteration

##### Iteration by Element

To iterate through each element inside an array, we can use loops.  

```rs
let arr: [i32; 5] = [1, 2, 3, 4, 5];

for element in &arr {
    println!("{}", element);
}
```

Here we print each element inside an array line by line.  
The variable `element` that we used in the for loop can actually be
named anything you like.  

##### Iteration by Index

Or we can use the more traditional way of accessing by indices.  

```rs
let arr: [i32; 5] = [1, 2, 3, 4, 5];

for i in 0..arr.len() {
    println!("{}", arr[i]);
}
```

Both outputs should be  

```
1
2
3
```

### Vector

Vectors are similar to arrays, except that they are resizeable.

#### Declaration

To create a vector, we can use the `Vec::new()` constructor method, or
call the `vec!` macro.  
To specify the type for variables, we use `Vec<T>` where
`T` is the type.  

> [!NOTE]
> The `T` in the `Vec<T>` is a way we use `Gerneric` in Rust. We call
> it `Gerneric Type Parameter` You will learn about this after
> midterm.  

For example:

```rs
let vec1: Vec<i32> = vec![1, 2, 3];     // method 1
let vec2: Vec<i32> = Vec::new();        // method 2
```

The difference between `vec!` and `Vec::new()` is that you can
initialize the vector with some items with `vec!` but not with
`Vec::new()`.  
`Vec::new()` will only create an empty vector, then you
can push in items later.

#### Vector Methods

To push in new elements, we use the `.push()` method. **Don't forget
to make the variable mutable.**  

```rs
let mut vec1: Vec<i32> = Vec::new();
vec1.push(5);
vec1.push(6);
vec1.push(7);
```

`vec1` will now contain 5, 6, 7.

To remove the last element, we can use `.pop()` method.  
`.pop()` also returns the popped element inside an option:  

```rs
Some(x)
None
```

So we can use `.unwrap()` to access that element.

> [!NOTE]
> `.unwrap()` is a method to simply access the value inside the
> `Option Enum`. We will talk about it later in the Enum topic later
> in today session.  

```rs
let mut vec: Vec<i32> = vec![5, 6, 7, 8];
let popped_num: i32 = vec.pop().unwrap();

println!("The popped num is: {}", popped_num);
```

The output should be  

```
The popped num is: 8
```

To access vector elements, we can use traditional indexing like
arrays, or we can use the `.get()` method for vectors.

    let vec1: Vec<i32> = vec![1, 2, 3];

    println!("{}", vec1[0]);
    println!("{}", vec1.get(0));

Both these results print out 1.
But using `.get()` will be safer, since if you use the traditional
indexing, you might index an element that doesn't exist, causing an
error.  

With `.get()`, the program will return these two options:  

```rs
Some(x)
None
```

Where `x` is the element that you are indexing.
If the element doesn't exist, the method will return `None`.  
If you get `Some(x)`, you can use a method like `.unwrap()` to get
the actual element.  

Like arrays, you can use `.len()` to get the length of a vector  

```rs
let vec: Vec<i32> = vec![1, 2, 3, 4, 5];

println!("The length of this vector is {}", vec.len());
```

The output should be  

```
The length of this vector is 5
```

#### Multidimensional Vector

We can also do `Multidimensional Vectors` same as arrays.

```rs
let mut vec: Vec<Vec<i32>> = vec![
    [1, 2, 3],
    [3, 2, 1]
];

println!("{}", vec.get(0).get(0));
```

The output should be  

```
1
```

#### Vector Iteration

To iterate through each elements, we can use loops.

```rs
let vec: Vec<i32> = vec![1, 2, 3];

for element in &vec {
    println!("{}", element);
}
```

The output should be  

```
1
2
3
```

## Tuple

A tuple is a general way of grouping together a number of values with
a variety of types into one compound type. Tuples have a fixed length.
Therefore, once declared, they cannot grow or shrink in size.

### Declaration

We create a tuple by writing a comma-separated list of values inside
parentheses. Each position in the tuple has a type, and the types of
the different values in the tuple don’t have to be the same.  
We can add optional type annotations to tuple variable.  

For example:  

```rs
let tup: (i32, String, u8, bool) = (67011352, String::from("Theepakorn Phayonrat"), 20, true);
// Assume this is my student information stores in tuple. (id, name, age, is_se_std)
```

We can  use pattern matching to destruct a tuple.  

For example:  

```rs
let coordinate: (i32, i32, i32) = (11, 45, -109);

let (x, y, z) = coordinate;
println!("My coordinate is X: {x}, Y: {y}, Z: {z}!");
```

The output should be  

```
My coordinate is X: 11, Y: 45, Z: -109!
```

### Accessing Element

We can also access a tuple element directly by using a period (`.`)
followed by the index of the value we want to access.

For example:

```rs
let lecturer: (String, u8, f32) = (String::from("Phairoj Jatanachai"), 58, 65000);

let name = lecturer.0;
let age = lecturer.1;
let salary = lecturer.2;

println!("<== Lecturer Information ==>");
println!("Name: {name}");
println!("Age: {age}");
println!("Salary: {salary}");
```

The output should be  

```
<== Lecturer Information ==>
Name: Phairoj Jatanachai
Age: 58
Salary: 65000
```

## Slice

In Rust, `Slices` are references to a portion of an array ot other
collection. Dynamically sized effience for working with subsets of
data.  
A `slice` is a kind of reference, so it does not have ownership.  

### Declaration

As mentioned earlier, `slices` are references of collection. We cannot
create one without referencing from other collection.  
Therefore, this how we normally create a `slice` variable.  

For example:  

```rs
let arr [i32; 5] = [1, 2, 3, 4, 5];
let slice = %arr[1..3] // From this line we're creating a slice of i32 from the 2nd to the 3rd element
                       // because of the same convention as indexing of array, excluding the end boundary.
println!("Here is your slice: {:?}", slice);
```

The output should be  

```
Here is your slice: [2, 3]
```

> [!NOTE]
> We will talk more aobut `slice` later on after the midterm. For now,
> we are using slice in for loop.  

> [!TIP]
> The `:?` inside the curly braces inside the `println!()` macro is the
> `Debug Format Specifier`. It is used to quickly print out those
> data from the Complex Data Types (that means we can use on all of
> them **IF THEY HAVE `#[derive(Debug)]` OR HAVE `Debug` TRAIT**). We will
> find out more in the real lecture.  

## &str and String

## Struct

A `struct`, or structure, is a custom data type that lets you package
together and name multiple related values that make up a meaningful
group. If you’re familiar with an object-oriented language, a struct
is like an object’s data attributes. In this section, we’ll compare
and contrast tuples with structs to build on what you already know and
demonstrate when structs are a better way to group data.  

## Enum

### Option Enum

### Result Enum
