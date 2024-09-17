---
layout: default
title: Rust Programming Language
parent: Programming
grand_parent: Fundamentals
nav_order: 1
tags: 
  - rust
---

# Rust Programming Language


About rust programming. I hope everyone to read [the official documentation](https://www.rust-lang.org), especially for installation of compilers for your computer. As the official documentation is well documented with the syntax, best practices, standard libraries and online console for practice.

This post includes from basic to advanced:

-------------------

1. [Hello, world!](#1-hello-world)   
2. [Basic syntax](#2-basic-synthax)  
2.1. [Immutables and mutables](#21-immutables-and-mutables)    
2.2. [Rust is a statically typed language](#22-rust-is-a-statically-typed-language)    
2.3. [Control Flow](#23-control-flow)     
2.4. [Functions](#24-functions)    
2.5. [Methods](#25-method)    
2.6. [Generics / Traits](#26-generics--traits)    
3. [Pointer / memory related features](#3-pointer--memory-related-features)        
3.1. [Memory in computer](#31-memory-in-computer)     
3.2. [Ownership](#32-owenership)      
3.3. [Pointers](#33-pointers)      
3.4. [Lifetimes](#34-lifetimes)    
4. [What's next?](#4-whats-next)    

-------------------

## 1. Hello, world!

First of all, `printing hello world!` in rust is as simple as in python:

```rust
fn main(){
    println!("Hello, world!");
}
```

But there are important things even in this simple code: 

1. RUST always needs main function named main().
2. We can define function with `fn`
3. `println!` is not a function, is a macro: macro is a piece of code that generates another piece of code.

> Note : **Macro**     
> Macros are expanded at compile time, so that the actual generated codes will replace the macro before the program is executed. When you want to use macro to generate code in compile level, you need to put `!` right after the name of macro.
> You can also make your codes be a macro. However, there is no free lunch. Macro makes you comfortable in writing a program or save execution time (by avoiding function call) but your compile time might also increase as much you use macro.  

And before talking about data structures and syntaxes of Rust, I will put the general naming rules.

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
One of the reason why we use Rust is for a memory-safe efficient programming. It is because that rust compiler strictly yells 
the programmer to follow their memory-safe instructions. 

### 2.1. Immutables and mutables  

In rust, `let` is used for declaration of a variable, without `mut` keyword, rust generally declare the variables be immutable by default. 

Basically, rust supports three ways to assign a value as follows : 

| Variable Type | Advantages | Disadvantages | Syntax
| --- | --- | --- |---|
| Mutable | Flexibility, efficiency | Thread safety, complexity | `let x = 3;` |
| Immutable | Thread safety, predictability, simplicity | Memory usage, performance | `let mut x = 3;` | 
| Constant | Readability, predictability, optimization | Limited flexibility, potential for code duplication | `const x = 3;` |

The following code snippet is from `the book` but with my commentary

#### 2.1.1. mutable vs immutable  
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

#### 2.1.2. constant
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

#### 2.1.3. shadowing   
Shadowing means that a variable is declared with the same name of previous variable.
I posted the usage because it is in `the book` of rust-lang, however, shadowing is not recommended in general cases.

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

Like modern programming languages like python or javascript, Rust supports type inference.   
However, rust generally requires concrete (static) types during compilation for memory-safe efficient programming. We call this type compliance as statically-typed language, which means the types of variables and expressions are checked at compile-time rather than at runtime.

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

### 2.5. Method

In rust, there is no `class` keyword, however, we can use `enum` and `struct` for OOP.   
If you are not familiar with reference (`&`) or dereference(`*`), I hope you to visit chapter 3 first then come back to this chapter.  
For instance, we can assign method with `impl` keyword as follows:

```rust
// based on an example from `the book`
struct Rectangle {
    width : u32,
    height: u32,
}
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
    fn radius_inscribe(&self) -> f64{
        let _width:f64 = self.width as f64;
        let _height:f64 = self.height as f64;

        f64::sqrt(_width.powf(2.0) + _height.powf(2.0))
    }
    fn has_larger_width_than(&self, other:&Rectangle) -> bool {
        // this function can get another argument : rectangle instance.
        self.width > other.width
    }

    // associated functions, we can instantiate with other way
    fn square(size:u32) -> Self {
        Self {
            width:size,
            height:size,
        }
    }
}
fn main(){
    let rect1 = Rectangle{
        width:30,
        height:40,
    };
    println!("the area of rectangle 1 is {}", rect1.area());
    println!("the radius of outer circle of rectangle {}", rect1.radius_inscribe());

    let rect2 = Rectangle{
        width: 10,
        height: 100,
    };

    println!("rect1 has larger width than rect2? : {}", 
            rect1.has_larger_width_than(&rect2));

    let square1 = Rectangle::square(30);
    println!("the area of square1 is {}", square1.area());
}
```

`enum` also can have methods, the following example is generated by copilot, 
which is also a good example that shows pattern matching.  

```rust
enum Direction {
    Up,
    Down,
    Left,
    Right,
}

impl Direction {
    fn is_vertical(&self) -> bool {
        match *self { // *self means dereference Direction, that gives a value.
            Direction::Up | Direction::Down => true,
            _ => false,
        }
    }

    fn is_horizontal(&self) -> bool {
        !self.is_vertical()
    }
}

fn main() {
    let up = Direction::Up;
    let left = Direction::Left;

    println!("Is 'up' vertical? {}", up.is_vertical()); // Prints: Is 'up' vertical? true
    println!("Is 'left' horizontal? {}", left.is_horizontal()); // Prints: Is 'left' horizontal? true
}
```

### 2.6. Generics / Traits

So far, we construct functions, enums and structs with strong type signatures or members. However, sometimes these strict way may induce duplications. For instance, let us define a function that give result of addtion of two numbers. 

```rust
fn add(a:f64, b:f64)->f64{
    a+b
}
fn main(){
    let a32:f32 = 3.0;
    let b32:f32 = 5.0;
    println!("{}", add(a32,b32)); 
    // we have error cuz we defined function `add` only with `f64`, not `f32` 
}
```

The above code causes compiler error. This is because the function can only takes `f64`. Even if we know that addition works with the same way with `i32` or `f32`, as we only defined add function only works for `f64`, the code has issues. To solve this problem we might need more add functions respect to each type. However this approach could result in redundancies in your code and it makes hard for you (or your team) to maintain the code. One of the idea to solve these redundant-prone coding is using `generics`. With generics, we can set a generic defintion of a function that can handles multiple types as followings:

```rust
fn add<T: std::ops::Add<Output= T>> (a :T, b : T ) -> T {
    a + b
}
fn main(){
    let a32:f32 = 3.0;
    let b32:f32 = 5.0;
    println!("{}", add(a32,b32));

    let a64:f64 = 4.0;
    let b64:f64 = 53.0;
    println!("{}", add(a64, b64));
}
```

Here, we set `T` as a abstract type, by putting angle bracket `< >` right next to the name of function. In this example, since we use standard add operator `+` we need to say that `T` has a special `trait` (or a constraint or characteristic) that this generic type `T` uses standard add operator's output. However, if we don't have those constraints (in this case, use of add-operator), The code snippet would be compiled.

The trait `std::ops::Add<Output=T>` is a powerful concept in Rust. Traits define functionality that types can implement. In this case, `std::ops::Add<Output=T>` constraint guarantees type T can be properly added within the function.

One of the interesting thing is that rust's generics don't impact performance.
The compiler generates specific code for each type at compile time, ensuring efficiency, resulting in performance equivalent to writing separate functions for each type.

Sometimes, we assume that the generalized members / signatures would not always be the same types. In this case, the generic type placeholder needs to be distinguished as followings:

```rust
struct Point <T,U>{
    x:T,
    y:U
}

fn main(){
    let x: i32 = 5;
    let y: f64 = 3.0;
    let pointxy = Point{x,y};
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

Ownership is one of the unique features in rust for handling memory not only within a code block (scope) but also between code blocks.

`the book` suggests three ownership rules as follows:

1. Each value in Rust has an owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

Because of axiom #2 and #3, programmers who are familiar with other languages could have in trouble.

For instance, in the case of python, the following would work.

```python
# python
if __name__ == "__main__":
    x = "hello"
    y = x
    print((id(x) == id(y))) # you can see True, they indicates the same memory address
    print(y) # you can get 'hello'
    print(x) # you can get 'hello'
```
with checking ids of x and y are equal, we can see x and y indicates the same address.

however, in rust programming, the following rust code  which is similar to the above code won't be compiled.

```rust
fn main(){
    let x = String::from("hello");
    let y = x;
    println!("{}", y);
    println!("{}", x); // compile error because x has no ownership of value
}
```

It seems that y and x "shares" the same address, however, it is not. As I mentioned first, in dealing with heap memory, rust basically moves the ownership from `x` to `y`.
As `y` has got ownership of the string value "hello", `x` cannot access the value until it retrieved ownership back or get new value assigned. 

### 3.3. Pointers
To solve the above ownership problem, we can also use other options : use memory address!  

#### 3.3.1. Reference

The foremost option is that borrow `x`'s value with `reference`.   
Here is an example of `borrowing` that y borrows the value from x, and return back automatically when y is called.   

```rust
fn main(){
    let x = String::from("hello");
    let y = &x; // y borrows (refers) the value of x; 
    println!("{}", y);
    println!("{}", x); // no error: Still, x is owner of "hello"
}
```

This `&` operator is for referring to x, or borrowing x address. It seems that reference is the same as the pointer in C, however 
references are always valid and automatically managed by Rust's borrow checker. **They cannot outlive the data they brought and cannot be null.**

You can see that those exmaples are based on `string` type because string in rust works allocating the memory based on move, not copy or clone.
If you use the same examples but `int` allocation, there is no error since the basic trait of allocation in `int` is value shallow copying.

The following code which seems like simply adding a dereference operator `*` could make you a little bit crazy.   
```rust
fn main() {
    let x = String::from("hello"); // we know, x is a string value "hello"; String
    let y = &x; // y borrows (refers) the value of x; &String
    let z = &*x; // (*x) is dereference of x, which is inner string (or string slice) : str 
                 // and then we refer &(*x), which is reference of string slice : &str

    println!("{}", y);
    println!("{}", x);
    println!("{}", z);
}
```  
I added the explanation of x, y and z in the code above with inline comment.

#### 3.3.2. Raw Pointer

Still, C-like pointer is also supported in Rust, named as `raw pointer`. 

Raw pointers in Rust are necessary for certain tasks where you need to interact with low-level code, 
such as interfacing with C libraries, implementing low-level data structures, or performing certain operations 
that can't be expressed safely within Rust's safe abstraction.

Here are some scenarios where raw pointers are useful:

1. **Interfacing with C Code**: Rust often needs to interact with C libraries, which typically use raw pointers extensively. Rust's FFI (Foreign Function Interface) allows you to call functions from C libraries and vice versa, and raw pointers are often used to pass data between Rust and C code.

2. **Unsafe Operations**: Some operations inherently require unsafe behavior, such as dereferencing a pointer to arbitrary memory or performing low-level memory manipulation. While these operations should be avoided whenever possible, there are situations where they're necessary for performance reasons or to implement certain algorithms.

3. **Unsafe Abstractions**: Sometimes, you may need to implement your own safe abstractions that rely on unsafe operations internally. While the interface exposed to the user remains safe, the implementation may use raw pointers or other unsafe constructs to achieve certain behaviors efficiently.

4. **Low-Level Data Structures**: Implementing low-level data structures like linked lists, trees, or graphs may require direct manipulation of memory addresses, which is facilitated by raw pointers. While Rust's standard library provides safe abstractions for common data structures, there are cases where custom implementations are necessary or desirable.

5. **Embedded Systems and Systems Programming**: In systems programming and embedded systems development, you often need precise control over memory layout and low-level hardware interactions. Raw pointers allow you to express such operations safely within the context of an `unsafe` block.

It's important to note that while raw pointers are a powerful tool, they come with significant responsibility. Rust's safety guarantees are designed to prevent common programming errors and security vulnerabilities, and bypassing these guarantees with raw pointers can introduce bugs, crashes, or security vulnerabilities if used incorrectly.

```rust
fn main() {
    let x = 42;

    // Reference to x
    let reference = &x;
    println!("Reference: {}", reference);

    // Raw pointer to x
    let raw_ptr: *const i32 = &x as *const i32;
    unsafe {
        println!("Raw pointer: {}", *raw_ptr);
    }
}
```

#### 3.3.3. Smart Pointer

Smart pointers in Rust are data structures that not only hold a value but also contain metadata and provide additional 
functionality beyond what regular pointers offer. 
They enforce various safety guarantees at compile time, ensuring memory safety and preventing common programming errors.

One of the most commonly used smart pointers in Rust is `Box<T>`. 
It allows you to allocate values on the heap rather than the stack and provides ownership semantics like any other value in Rust. 
Here's a brief overview of `Box<T>`:

- **Box\<T\>**:  Box is a smart pointer that owns the data it points to and is stored on the heap. 
It's used when you need to have a value with a known size at compile time but don't know the precise size until runtime, 
or when you want to transfer ownership of a value across scopes or threads.

Here's a simple example of `Box<T>`:

```rust
fn main() {
    let x = Box::new(42); // Allocate an integer on the heap
    println!("Value: {}", x); // Print the value stored in the Box
}
```

In addition to `Box<T>`, Rust provides other smart pointers like `Rc<T>` and `Arc<T>` for shared ownership, 
`Cell<T>` and `RefCell<T>` for interior mutability, and `Mutex<T>` and `RwLock<T>` for synchronization, among others. 
Each smart pointer type has its own characteristics and use cases, allowing you to choose the appropriate one based on your requirements.

Smart pointers enable you to write safer, more expressive code by encapsulating complex memory management logic and providing clear ownership semantics. 


### 3.4. Lifetimes

The following's a code from `the book`.

```rust
fn longest(x: &str, y:&str) -> &str {
    if x.len() >= y.len() {
        return x;
    }
    else {
        return y;
    }
}

fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

```

If you compile the code above, you will get an error

```sh
error[E0106]: missing lifetime specifier
 --> src/main.rs:1:32
  |
1 | fn longest(x: &str, y:&str) -> &str {
  |               ----    ----     ^ expected named lifetime parameter
  |
  = help: this function's return type contains a borrowed value, but the signature does not say whether it is borrowed from `x` or `y`
help: consider introducing a named lifetime parameter
  |
1 | fn longest<'a>(x: &'a str, y:&'a str) -> &'a str {
  |           ++++     ++         ++          ++

For more information about this error, try `rustc --explain E0106`.
```

what is a lifetime?

As we mentioned in [3.1.3. reference](#331-reference), rust compiler prevent the reference outlive the data by using borrow checker : so to speak, try to prevent `dangling pointer`.

With this function of references signatures, rust compiler worry about the rest of reference. If x is returned, what about y? y could become a dangling pointer. vice versa.

Therefore, we need to notify to compiler (actually for ourselves) that the rest reference will be terminated in the same lifetime of the picked (returned) one.

In function `longest`, returning a reference to x would invalidate y (become a dangling reference) if x was longer. Lifetimes prevent this by making the references' validity explicit.

The fixed version is as follows:

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() >= y.len() {
        return x;
    } else {
        return y;
    }
}
```

-------------------

## 4. What's next?

It seems that we only discuss basic syntax, memory related features ... What's next?       
You might think that you didn't finish at all. Because we didn't discuss modules and crates, I/O, threading, standard libraries, handling errors and so on!

Those subjects are different from the subjects we've discussed. So far, we only focus on the distinct and basic characteristics of rust programming language. The subjects we haven't discucssed, actually, are not recommended to be learned only by reading. From now on, we need to start...


<div 
  align="center" 
  style="
  font-size:300%;
  text-decoration-line: underline;
  text-decoration-style: double;
  text-decoration-thickness: 5px; 
  "
>
LEARNING BY DOING!
</div>