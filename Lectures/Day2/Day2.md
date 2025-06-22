# Topics

- Basic Ownership
    - Rules of Ownership
    - Reference
    - Mutable Reference VS Immutable Reference
    - Dangling Reference
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
        - Defining and Instantiating
        - Accessing Attribute
        - Debugging Struct
        - Struct-Returning Function
    - Enum
        - Defining and Instantiating
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

# Complex Data Types

In Rust, the word `Complex Data Types` is not officially defined but it
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

## String and &str

In Rust, there 

### String

#### Declaration

In Rust, we can declare `String` with these 2 `String constructor methods`.  

```rs
let empty_string: String = String::new(); // To create empty String
let some_string: String = String::from("Some String"); // To create non-empty String
```

### Accessing Character

To access each character in String, we cannot simply use index
directly like array or vector.  

### String Methods

There are also some `String Methods` you should know too.  

For example:  

```rs
let empty_string: String = String::new(); // To create empty String

if empty_string.is_empty() { // To check whether the String is empty or not
    println!("The String is empty!");
} else {
    println!("The String is NOT empty!");
}
```

The output should be  

```
The String is empty!
```

### &str

### String VS &str

### format!() Macro

In Rust, there is a macro to fotmat `Strings`.

For example:  

```rs
let hello: String = String::from("Hello");
let world: String = String::from("World");
let hello_world: String = format!("{}, {}!", hello, world);

println!("{hello_world}");
```

The output should be  

```
Hello, World!
```

## Struct

A `struct`, or structure, is a custom data type that lets you package
together and name multiple related values that make up a meaningful
group. If you’re familiar with an object-oriented language, a `struct`
is like an object’s data attributes. In this section, we’ll compare
and contrast `tuples` with `structs` to build on what you already know
and demonstrate when `structs` are a better way to group data.  

`Structs` are similar to tuples, discussed in `Tuple` section, in that
both hold multiple related values. Like tuples, the pieces of a struct
can be different types. Unlike with `tuples`, in a `struct` you’ll
name each piece of data so it’s clear what the values mean.  
Adding these names means that structs are more flexible than tuples:
you don’t have to rely on the order of the data to specify or access
the values of an instance.  

### Defining and Instantiating

To define a `struct`, we enter the keyword `struct` and name the
entire `struct`. A struct’s name should describe the significance of
the pieces of data being grouped together.  
Then, inside curly brackets, we define the names and types of the
pieces of data, which we call fields.

For example:  

```rs
struct Player {
    name: String,
    id: u32,
    level: u8,
    is_in_quest: bool,
}
```

> [!IMPORTANT]
> The naming convention of `Struct names`, `Enum names`, `Trait names`
> and `Type aliases` are `Pascal Case`, which can be written like
> this: `PascalCase`  

To use a `struct` after we’ve defined it, we create an instance of
that `struct` by specifying `concrete values` for each of the fields.  
We create an instance by stating the name of the `struct` and then add
curly brackets containing `key: value` pairs, where the `keys` are
the `names` of the fields and the `values` are the `data` we want to
store in those fields.  
We don’t have to specify the fields in the same
order in which we declared them in the `struct`. In other words,
the `struct` definition is like a general template for the type, and
instances fill in that template with particular data to create values
of the type.  

For example:  

```rs
let p1 = Player {
    name: String::from("Pottarr"),
    id: 67011352,
    level: 100,
    is_in_quest: true,
};

### Accessing Attribute
```

To get a specific value from a `struct`, we use dot notation.  
For example, to access this player’s name, we use `p1.name`.  
If the instance is mutable, we can change a value by using the dot
notation and assigning into a particular field.  

For example:  

```rs
struct Player {
    name: String,
    id: u32,
    level: u8,
    is_in_quest: bool,
}

let mut p1 = Player {
    name: String::from("Pottarr"),
    id: 67011352,
    level: 100,
    is_in_quest: true,
};

p1.name = String::from("KrakenLord");
p1.id = 67011090;
```

From now, the p1's name attribute is now `String::from("KrakenLord")`
and also applies to the p1's `id` attribute which is now `67011090`.

Note that the entire instance must be mutable; Rust doesn’t allow us
to mark only certain fields as mutable.  

For example:  

```rs
struct Player {
    mut name: String,
    id: u32,
}

fn main() {
    let mut p1 = Player {
        name: String::from("Pottarr"),
        id: 67011352,
    };
    p1.name = String::from("KrakenLord");
}
```

The output should be  

```
   Compiling playground v0.0.1 (/playground)
error: expected identifier, found keyword `mut`
 --> src/main.rs:2:5
  |
1 | struct Player {
  |        ------ while parsing this struct
2 |     mut name: String,
  |     ^^^ expected identifier, found keyword

error: could not compile `playground` (bin "playground") due to 1 previous error
```

From this pile of code, Rust expected an `identifier`, not a `keyword`
from your struct definition.  
Siply `let mut` the `p1` instance is enough to make the attributes
mutable. We don't need to use `mut` inside the `struct` definition.

### Debugging Struct

As we were told earlier that when we use `#[derive(Debug)]` on any
struct written by yourselves (AKA non-std library defined struct), it
doesn't automatically have `Debug` trait implemented.  
So if you want to debug print the `struct`, make sure to add
`#[derive(Debug)]` above the struct definition.  

For example:  

```rs
struct Player {
    name: String,
    id: u32,
    level: u8,
    is_in_quest: bool,
}

let mut p1 = Player {
    name: String::from("Pottarr"),
    id: 67011352,
    level: 100,
    is_in_quest: true,
};

println!("{:?}", p1);

p1.name = String::from("KrakenLord");
p1.id = 67011090;

println!("{:?}", p1);
```

The output should be  

```
Player { name: "Pottarr", id: 67011352 }
Player { name: "KrakenLord", id: 67011090 }
```

### Struct-Returning Function

As with any expression, we can
construct a new instance of the struct as the last expression in the
function body to implicitly return that new instance.

For example:  

```rs
fn create_player(name: String, id:32) -> Player {
    Player {
        name: name,
        id: id,
        level: 1,
        is_in_quest: false,
    }
}
```

This function will take arguments and return a new `Player` instance.

> [!TIP]
> As you can see, the parameter names and struct attribute identifiers
> can have the same name.

Also, there is another way you can shorthand this function by this
example below:  

```rs
fn create_player(name: String, id:32) -> Player {
    Player {
        name,
        id,
        level: 1,
        is_in_quest: false,
    }
}
```

Since the parameters name is the same as the struct attribute
identifiers, we can write `name` rather than `name: name`.

> [!NOTE]
> We will talk more about `struct` in te real lectures where the
> professor will teach you about the Struct implementation or `impl`

## Enum

In this chapter, we’ll look at `enumerations`, also referred to as
`enums`. Enums allow you to define a type by enumerating its possible
variants. First we’ll define and use an enum to show how an enum can
encode meaning along with data.  

### Defining and Instantiating

In this case we're using traffic light as our visualization. Imagine
we want to present a traffic light than can either be **Red**,
**Yellow**, or **Green**.  

We can write it as a Rust Enum like this:  

```rs
enum TrafficLight {
    Red,
    Yellow,
    Green,
}
```

Now, with this `TrafficLight` enum, we can write a function that tell
us what to do when encountering these color lights.  

For example:  

```rs
fn traffic_instruction(light: TrafficLight) {
    match light {
        TrafficLight::Red => println!("Stop!"),
        TrafficLight::Yellow => println!("Slow Down!"),
        TrafficLight::Green => println!("Go!"),
    }
}
```

As we combine all codes we'll get a simple traffic light program:  

```rs
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

fn main() {
    traffic_instruction(TrafficLight::Green);
}

fn traffic_instruction(light: TrafficLight) {
    match light {
        TrafficLight::Red => println!("Stop!"),
        TrafficLight::Yellow => println!("Slow Down!"),
        TrafficLight::Green => println!("Go!"),
    }
}
```

The output should be  

```
Go!
```


### Standard Library Enum

`Standard Library Enums` are enum types that are defined and provided
by Rust's Standard Library (`std`). From this you can use it without
writing it yourselves or importing external crates.  
They are parts of built-in tools that come with Rust.  

#### Benefits

By the common nature of Rust, it does not have `null` or `execeptions`.
Rust wants to make `absences of a value` and `error handling`
**explicit** and **safe**. Therefore, you must handle them properly.
This is why STD Lib Enums come to be a part of your programs.  

#### Option

`Option<T>` is used when a value **might or might not exist**.  

Here is how Option Enum is defined:  

```rs
enum Option<T> {
    Some<T>,
    None,
}
```

We can demonstrate the Option Enum by this simple function.

For example:  

```rs
fn main() {
    match someone(1) {
        Some(x) => println!("Some: {x}"),
        None => println!("None"),
    }
    match someone(2) {
        Some(x) => println!("Some: {x}"),
        None => println!("None"),
    }
}

fn someone(one: i32) -> Option<i32> {
    // Will return Some(1) when received 1 and return None in other cases.
    if one == 1 {
        Some(1)
    } else {
        None
    }
}
```

The output should be  

```
Some: 1
None
```

From Vector, we've learnt that the method `.get()` returns `Option<T>`.  

We can use it like this:  

```rs
fn main() {
    let db = vec!(("SnackJek", 15.00), ("TofuBoy", 20.00), ("Isded", 15.00));
    check_existence(&db, 5);
    check_existence(&db, 1);
}

fn check_existence(vec:&Vec<(&str, f64)>, target: usize) {
    match vec.get(target) {
        Some((name, price)) => println!("Found product name: {} with price = {:.2}THB in INDEX = {}", name, price, target),
        None => println!("No product with INDEX = {} exists!", target),
    }
}
```

The output should be  

```
No product with INDEX = 5 exists!
Found product name: TofuBoy with price = 20.00THB in INDEX = 1
```

#### Result

Result<T, E> is used when an operation can succedd ro fail.

This is how Result Enum is defined:  

```rs
enum Result<T, E> {
    Ok<T>,
    Err<E>,
}
```

We all know that we cannot divided by zero. Therefore we can prevent
runtime error by using `Result Enum`.  

We can use it like this:  

```rs
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Division by zero"))
    } else {
        Ok(a / b)
    }
}

fn main() {
    match divide(10.0, 2.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
    match divide(3.0, 0.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
}
```

The output should be  

```
Result: 5
Error: Division by zero
```

---

Back to [Main Page](../../README.md)

---

Lecturers:  
[Pottarr](https://github.com/Pottarr) (P'Potter)  
[Krakenlord5](https://github.com/Krakenlord5) (P'Tang)
