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

About rust programming. I hope everyone to read [the official documentation](https://www.rust-lang.org). As it is well documented with the best practices, I assume that you can easily figure out most of things in RUST.

This post includes very from basic to advanced:

- of course, hello world!
- overall syntax
- pointer / memory related features
- sync / async programming 
- building a simple physics engine.

## Of course, Hello World!

First of all, it is very simple as python for printing hello world:

```rust
fn main(){
    println!("hello world");
}
```

But there are important things in this simple code: 

1. RUST always needs main function named main().
2. We can define function with `fn`
3. `println!` is not a function, is a macro: macro is a piece of code that generates another piece of code.

> Note : Macro
> Macros are expanded at compile time, so that the actual generated code will replace the macro before the program is executed. when you want to use macro to generate code in compile level, you need to put `!` right after the name of macro.

## Basic Synthax
### RUST is static typed language.
Comming Soon
{: .label .label-yellow}