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

1. of course, "Hello, world!"
2. overall syntax
3. pointer / memory related features
4. sync / async programming 
5. building a simple physics engine.

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

## Basic Synthax
One of the reason why we use Rust is for a memory-safe efficient programming

### Immutables and mutables  

Like I've said, Rust targets to a memory-safe efficient programming. For safety, Rust sets variable as an immutable by default. 

Basically, Rust supports three ways to assign a value as follows : 

| Variable Type | Advantages | Disadvantages | Syntax
| --- | --- | --- |---|
| Mutable | Flexibility, efficiency | Thread safety, complexity | `let x = 3;` |
| Immutable | Thread safety, predictability, simplicity | Memory usage, performance | `let mut x = 3;` | 
| Constant | Readability, predictability, optimization | Limited flexibility, potential for code duplication | `const x = 3;` |

### Rust is a statically-typed language.
Comming Soon
{: .label .label-yellow}  

Like modern object-oriented programming like python or javascript, Rust supports type inference.   
However, Rust generally requires concrete (static) types during compilation for memory-safe efficient programming. We call this type compliance as statically-typed language, which means the types of variables and expressions are checked at compile-time rather than at runtime.

```rust
fn main(){
    let x : i32 = 5;
}
```
1. The typing i32 means immutable variable x is 32bit-integer type (with sign) and initial value is 5.
