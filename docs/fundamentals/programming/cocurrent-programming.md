---
layout: default
title: Concurrent / Parallel Programming
parent: Programming
grand_parent: Fundamentals
nav_order: 1
tags: 
  - concurrent programming
  - parallel computing
  - rust
---

# Concurrent / Parallel Programming  
Introduction and Term (23.07.13)
{: .label .label-green}

Concurrency and Parallelism(TBD)
{: .label .label-yellow}

## Introduction

When a top-level programmer encounters a time-efficiency issue in an algorithm, program, or system, it is natural for them to think of divide-and-conquer first. As systems grow in size and demand for speed increases, the ways in which divide-and-conquer is used have evolved alongside the work of top-level software researchers.

Concurrent / parallel programming is a recent paradigm of divide-and-conquer that is being used in practice.

With this post, I hope the readers (also myself) to study the concurrent / parallel programming thoroughly with Rust.

## Prerequisite (Terminology)

The following table is a basic terminology about process and thread. Even if you are not interested in concurrent / parallel programming, if you're a software engineer, you need to know those things.

| Term | Description |
|---|---|
| **Process** | A program in execution. It is an instance of a program that is running on a computer. |
| **Thread** | A lightweight process. It is a sequence of instructions that can be executed independently of other threads. |
| **Process ID (PID)** | A unique identifier that is assigned to each process. It is used to identify the process and to distinguish it from other processes. |
| **Thread ID (TID)** | A unique identifier that is assigned to each thread. It is used to identify the thread and to distinguish it from other threads. |
| **Parent process** | A process that creates another process. The created process is called the child process. |
| **Child process** | A process that is created by another process. The creating process is called the parent process. |
| **Kernel** | The core of the operating system. It is responsible for managing processes and other resources. |
| **Scheduler** | The scheduler is responsible for scheduling processes to run on the CPU. |
| **Context switching** | The process of switching from one process to another. |
| **Inter-process communication (IPC)** | IPC is the process of communication between processes. |
| **register** | A small, fast memory that is used to store data that is being used by the CPU (for frequent uses)|
| **Program counter** | A register that stores the address of the next instruction that the CPU will execute |   

### More Specific Definition on Process
From the table above, a process is a program in execution. It means it has not only a space aspect (a program) but also time aspect (in execution)

A process's space is the allocated memory. This memory includes the code of the program, the data using for the program, and the stack. Here, the stack is used to store the state of the process, such as the values of the registers and the program counter

> **Note**  
 <u>stack</u> is a linear LIFO (Last-In First-Out) data structure.

In terms of time aspect, the time includes the process **waiting** time for its running, process **running** time, and its **waiting I/O** time.

These aspects of a process are closely related. the allocated memory of a process determines how long the process can run. vice versa.

The understanding of those aspects also helpful for understanding how processes interact with each other (concurrent / parallel programming). When two processes share memory, they must be careful not to overwrite each other's data. And when two processes communicate with each other, they must be careful not to send each other too much data.

### More Specific Definition on Thread

A thread is the smallest sequence of instructions that can be executed not depending on the others. Each thread has its own stack and registers, however, it shares the same memory with others in the same process. It means that threads can access the same data, but they cannot modify each other's data without synchronization.

