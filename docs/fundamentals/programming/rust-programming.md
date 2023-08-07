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
Initialized (23.07.18)
{: .label .label-green}

About rust programming. I hope everyone to read [the official documentation](https://www.rust-lang.org), especially for installation of compilers for your computer. As the official documentation is well documented with the syntax, best practices, standard libraries and online console for practice.

This post includes from basic to advanced:

1. Of course, "Hello, world!"
2. Basic syntax
3. Pointer / memory related features
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
| Types / Traits / Enum variants | PascalCase |
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
    let ch_char : char = 'c';

    // about tuple
    let compound_tup : (i32, f64, u16) = (400, 6.28, 2);
    let four_hundred = compound_tup.0;
    let tau = compound_tup.1;
    let two = x.2;

    // about array
    let arr_1 = [1,2,3,4,5];
    let arr_2 : [i32; 5] = [1,2,3,4,5]; // 
    let arr_3 = [3; 5]; // [3,3,3,3,3]
    let arr_first = arr_1[0];
    let arr_second = arr_1[1];
}
```

#### 2.2.2. Custom Types

In progress (23.08.07)
{: .label .label-yellow}
