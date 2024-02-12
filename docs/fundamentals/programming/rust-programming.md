---
layout: default
title: Rust Programming Language
parent: Programming
grand_parent: Fundamentals
nav_order: 2
tags: 
  - rust
---

# Rust Programming Language
Update 2.2.2 Custom Types - Enum (Feb.10.2024)
{: .label .label-purple}


About rust programming. I hope everyone to read [the official documentation](https://www.rust-lang.org), especially for installation of compilers for your computer. As the official documentation is well documented with the syntax, best practices, standard libraries and online console for practice.

This post includes from basic to advanced:

-------------------

1. [Of course, "Hello, world!"](#1-of-course-"hello-world")   
2. [Basic syntax](#2-basic-synthax)  
2.1. [Immutables and mutables](#21-immutables-and-mutables)    
2.2. [Rust is a statically typed language](#22-rust-is-a-statically-typed-language)    
2.3. [Control Flow](#23-control-flow)    
2.4. [Functions](#24-functions)
3. [Pointer / memory related features](#3-pointer--memory-related-features)      
3.1. [Memory in computer](#31-memory-in-computer)     
3.2. [Ownership](#32-owenership)     
3.3. Pointers   
3.4. Lifetimes
4. Sync / async programming 
5. Building a simple physics engine.

-------------------

## 1. Of course, "Hello, world!"

First of all, it is very simple as python for printing hello world:

```rust
fn main(){
    println!("Hello, world!");
}
```

But there are important things in this simple code: 

1. RUST always needs main function named main().
2. We can define function with `fn`
3. `println!` is not a function, is a macro: macro is a piece of code that generates another piece of code.

> Note : **Macro**     
> Macros are expanded at compile time, so that the actual generated codes will replace the macro before the program is executed. When you want to use macro to generate code in compile level, you need to put `!` right after the name of macro.
> You can also make your codes be a macro. However, there is no free lunch. Macro makes you comfortable in writing a program or save execution time (by avoiding function call) but your compile time might also increase as much you use macro.  

Before talking about data structures and syntaxes of Rust, I will put the general naming rules.

| Item | Convention |
| --- | --- |
| Modules | snake_case |
| Types / Traits / Enum / Struct | PascalCase |
| Macros / Functions / Methods | snake_case |
| Local variables | snake_case |
| Static variables / Constants | SCREAMING_SNAKE_CASE |
| Type parameters | usually single uppercase letter: T or consice PascalCase |
| Lifetimes | usually a single letter: 'a or short lowercase : 'de, 'src |

-------------------

## 2. Basic Synthax
One of the reason why we use Rust is for a memory-safe efficient programming

### 2.1. Immutables and mutables  

Like I've said, Rust targets to a memory-safe efficient programming. For safety, Rust sets variable as an immutable by default. 

Basically, Rust supports three ways to assign a value as follows : 

| Variable Type | Advantages | Disadvantages | Syntax
| --- | --- | --- |---|
| Mutable | Flexibility, efficiency | Thread safety, complexity | `let x = 3;` |
| Immutable | Thread safety, predictability, simplicity | Memory usage, performance | `let mut x = 3;` | 
| Constant | Readability, predictability, optimization | Limited flexibility, potential for code duplication | `const x = 3;` |

The following code is from official rust language book but with my commentary

**mutable vs immutable**
```rust
// the following code occurs compile error, this example is from `the book` of rust-lang.org

fn main(){
    let x = 5; // rust sets value be immutable by default. (1)
    println!("the value of x is {x}");
    x = 6; // here the mutation occurs however as we did not set x be mutable, this occurs compile error in rust.
    println!("the value of x is {x}"); //compiler cannot reach here.
}

```

1. To avoid the error, we need to write (1) as `let mut x = 5;`

**constant**
```rust
fn main(){
    const mut X = 5; // compile error occurs here, unlike `let` expression which initializes variables, `const` does not allow `mut` expression.
}
```
2. Compiler might tell you change `const` to `static`. However, even if you change it, the expression of `static mut` is unsafe, so you get compiler error again with : `error[E0133]: use of mutable static is unsafe and requires unsafe function or block`

3. This error (`[E0133]`) can be avoided with unsafe block (which allows memory-unsafe coding) like the following, however, you might not need this usages right now. (not recommended)

```rust
fn main() {
    unsafe{
        static mut A:i32 = 1024; // static needs the concrete type like i32 here.
        println!("Hello, world!");
        println!("{A}");
    }
}
```

**shadowing**
Shadow means that a variable is declared with the same name of previous variable.
I am posting the usage because it is in `the book` of rust-lang, however, shadowing is not recommended in general cases.

```rust
fn main(){
    let x = 5;
    let x = x + 1; // shadow variable x to be x (prev) + 1 : 6
    {
        let x = x * 2; // inner shadow of x : 
    } // shadowing scope ends here. so the value of the variable x is now 6,
}
```

1. Unlike mutable variable, we can change the value of variable in compile-time.

2. Unlike mutable variable, shadowing allows to use the same name when we change the data type. This sounds like powerful. However, type-changing situation sometimes leads a serious dynamic errors that compiler cannot discern. Therefore, shadowing should be avoided in general cases.

### 2.2. Rust is a statically-typed language.

Like modern object-oriented programming like python or javascript, Rust supports type inference.   
However, Rust generally requires concrete (static) types during compilation for memory-safe efficient programming. We call this type compliance as statically-typed language, which means the types of variables and expressions are checked at compile-time rather than at runtime.

#### 2.2.1. Basic Scalar Types

Here are the table for Rust's basic types

 | Type | Description | Literal |
 | --- | --- | --- |
 | i8 \~ i128 | 8 \~ 128 bit integer |  | 
 | u8 \~ i128 | 8 \~ 128 bit unsigned integer |  |
 | isize | integer depends on the architecture |  | 
 | usize | unsigned integer depends on the architecture |  |
 | f32 \~ f64 | 32 \~ 64 bit floating points | 3.1415 |
 | bool | boolean type | true / false |
 | char | letter type | 'a' |
 | (type, type, ... ) | tuple | (true, 5) |
 | [type; integer value ] | array | [3; 10] | 

Here the following codes are about declaration of types: 
```rust
fn main(){
    let five = 5 ; // Rust basically infers integer value as i32.
    let five_i32 : i32 = 5; // i32
    let pi = 3.14; // Rust baically infers float value as f64
    let pi_f32 : f32 = 3.14; // f32
    let t = true; // bool
    let f : bool =  false; // bool
    let ch = 'c';
    let ch_char : char = 'c'; // char is 32 bit length

    // about tuple
    let compound_tup : (i32, f64, u16) = (400, 6.28, 2);
    let four_hundred = compound_tup.0;
    let tau = compound_tup.1;
    let two = compound_tup.2;

    // about array
    let arr_1 = [1,2,3,4,5];
    let arr_2 : [i32; 5] = [1,2,3,4,5]; // 
    let arr_3 = [3; 5]; // [3,3,3,3,3]
    let arr_first = arr_1[0];
    let arr_second = arr_1[1];
}
```

#### 2.2.2. Custom Types

Rust also supports algebraic types. `struct` can be used as a `multiple type`, `enum` can be used as a `sum type`.

**Struct**

Like C programming language, Rust has a `struct` type that user can define a custom data. While rust's struct is more expansive than C struct, 
it enables users to use flexible and memory-safe custom data.

```rust
struct Person{
    name: String,
    age: u8,
}

fn main(){
    let person = Person{
        name:"Sangdo Han".to_string(),
        age:33,
    };
}
```

**Enums**

Enums, which stands for enumerations, is one of the powerful custom type that enables to make custom types in modern programming language.
Briefly speaking, enums gives user to selecting a value of a possible set of values. With this concept, users can write more safe and expressive codes.

```rust
enum OrderStatus{
    Pending,
    Approved,
    Processing,
    Shipped,
    Delievered,
    Canceled,
}
struct Order{
    id: u32,
    customer_name: String,
    items: Vec<String>,
    status: OrderStatus,
}
fn main(){
    let mut order = Order{
        id:1,
        customer_name: "Sangdo Han".to_string(),
        items: vec!["TV".to_string(), "Laptop".to_string()],
        status:OrderStatus::Pending,
    };

    order.status = OrderStatus::Processing;
}
```

Like the example above, programmer can set the `Order`'s status using enum `OrderStatus` with the follwing **valid** variables :
`Pending`, `Approved`, `Procesing`, `Shipped`, `Delivered` and `Canceled`.

These options might have different types and amounts of associated data. Enums with inline-struct can make more properous types as followings:

```rust
use chrono::{DateTime, Local, TimeZone, Utc};

#[derive(Debug)]
enum OrderStatus{
    Pending,
    Approved{start_date:DateTime<Utc>, approver:String},
    Processing{start_date:DateTime<Utc>, provider:String},
    Shipped{start_date:DateTime<Utc>, ship_no:u32},
    Delievered{start_date:DateTime<Utc>, expected_date:DateTime<Utc>},
    Canceled
}

#[derive(Debug)]
struct Order{
    id: u32,
    customer_name: String,
    items: Vec<String>,
    status: OrderStatus,
}

fn main(){
    let mut order = Order{
        id:1,
        customer_name: "Sangdo Han".to_string(),
        items: vec!["TV".to_string(), "Laptop".to_string()],
        status:OrderStatus::Pending,
    };

    order.status = OrderStatus::Processing{start_date:Utc::now(), provider:"sangdo".to_string()};
    println!("{:?}", order)
}
```

#### 2.2.3. Common collections
Rust's standard library supports useful data structures called collections.
It supports `Vec` (vector), `VecDeque` (queue), `HashMap` and so on.
<a href="https://doc.rust-lang.org/std/collections/index.html">
   see the details in the official documentation of std::collections
</a>

### 2.3. Control Flow

In any programming language, if you know if-else and loop, you can write any program even if it is too hard to read or too slow.

#### 2.3.1. if expression

Rust's `if` is an expression, so it returns value. If there is no explicit `return` statement, it automatically returns `Unit Type` (`()`), which represents an empty tuple.

As `if` is an expression, we can assign value as the following:
```rust
let result = if condition { 
        value_1 ;
    } else if condition_2 {
        value_2 ;
    } else {
        value_3 ;
}
```
about the conditions, rust only supports boolean type.

#### 2.3.2. pattern matching    

Rust has a powerful control flow construct keyword : `match` expression.
Basically, match works as the following:
```rust
match expression {
    pattern1 => { code_block1 },
    pattern2 => { code_block2 },
    // ..
}
```
For a simple example, you can register patterns with literals and wildcard (`_`) as the following:
```rust
let dice_roll = 3;
match dice_roll {
    3 => println!("you got the prize : candies !"),
    5 => println!("you got the prize : chocolate !"),
    _ => println!("try again")
}
```

Generally, enum is widely used for pattern constraints. the following example is originated from `the book`.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```


#### 2.3.3. loop, while, for
1. loop    
`loop` is basically means `while true` in rust, unlike `while` or `for`, however, it can be used as expression with `break`. In addition, rust can assign a label to a loop as followings:
```rust
'outer_loop: loop {
    'inner_loop: loop {
        // ...
        if some_condition {
            break 'outer_loop;  // with label, loop can exit directly to outer loop
        }
    }
}
```
2. while    
`while` is a repitition keyword as widely used in other programming language. With a condition phrase beside `while`, we can control the repetition. `while` needs discrimitive condition to break the repetition.
```rust
while some_condition {
    // ... 
    if other_condition {
        break;
    }
}
```
3. for    
`for` could be the best option that handling the repetition with fixed range.
```rust
let mut factorial = 1;
    for idx in 1..10 { // starts 1 to 9, if you want to include 10, use 1..=10
        factorial *= idx;
        println!("{idx}! = {factorial}");
}
```


### 2.4. functions

We've already used a function : `main`. As you might know already, to make a function, the keyword `fn` is needed. Also, the function named `main` is a crucial function, that rust compiler identifies the project. So far, we don't put the parameters for the function. To give a parameters to a function, we have some rules as follows:

```rust
fn make_2d_circle(x:f64, y:f64, r:f64){
    assert!( r>0.0 , "r needs to be larger than 0 but you put : {}",r);
    println!("circle created at ({x}, {y}) with radius {r}");
}

fn main(){
    let inputs : (f64, f64, f64) = (2.0, 3.0, 2.0);
    make_2d_circle(inputs.0, inputs.1, inputs.2);
}
```

with the outputs, we should declare return type.

```rust
fn calculate_polar_coordinates(original_x: f64, original_y: f64) -> (f64, f64) {
    // Calculate radius using the Pythagorean theorem
    let polar_radius = (original_x.powf(2.0) + original_y.powf(2.0)).sqrt();

    // Calculate theta using atan2 (handles 0/0 case)
    let polar_theta = original_y.atan2(original_x);

    (polar_radius, polar_theta) // return (polar_radius, polar_theta);
}

fn main() {
    let original_x = 2.0;
    let original_y = 3.0;

    let (polar_radius, polar_theta) = calculate_polar_coordinates(original_x, original_y);

    println!("Original x: {}, y: {}", original_x, original_y);
    println!("Converted to Polar Coordinates:");
    println!("Radius: {}, Theta: {}", polar_radius, polar_theta);
}
```

-------------------

## 3. Pointer / memory related features

### 3.1 Memory in Computer
In operating system, memory (RAM) is divided into 5 parts (regions, segments): Text (code), bss, data, heap and stack. Those manage memory in runtime.

<div align="center">
<p>
<image src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Program_memory_layout.pdf/page1-234px-Program_memory_layout.pdf.jpg" />
</p>
 ref: data segment image <a href="https://en.wikipedia.org/wiki/Data_segment"> from wikipedia </a>
</div>

1. Text (code)  
Text stores executable code, generally fixed size.

2. Data    
Data stores initialized global variables and `static` variables.

3. BSS (Block Started by Symbol)    
BSS generally depends on Programming language, however, like C programming language, manages unintialized static variable.

4. Stack    
Stack is a LIFO (last-in first-out) data structure, using a mechanism like a piling up pancakes on a plate. Stack manages function calls, local variables, function parameters, and return addresses. Stack pointer is a special register in the CPU keeps track of the top of the stack, pointing the current memory address where new data can be added(malloc), also deleting(free) the topmost data easily. 
- When a function is called, a new stack frame is created on the stack to hold its local variables, parameters and return address.
- For a memory allocation, compiler analyzes the code to determine the size of each function's stack frame. Next, compiler generates instructions to adjust the stack pointer, allocating and dellocating space as needed.
- function calls and returns automatically add (push) and remove (pop) stack frames, ensuring efficient memory usage.

5. Heap  
Heap is a kind of less organized region of memory, which can be manually directed by user. In the sense of automatic managing, 
many languages support `garbage collection` in these days. Rust does not have a traditional garbage collector. Instead, 
it relies on ownership and borrowing. Rust also supports pointers for user's memory manipulation, some of them is safer than those in C / C++.

### 3.2. Owenership


TBD 
{: .label .label-yellow}      


Ownership is one of unique features in rust for handling memory not only within a code block (scope) but also between code blocks.

`the book` suggests three ownership rules as follows:

1. Each value in Rust has an owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.
