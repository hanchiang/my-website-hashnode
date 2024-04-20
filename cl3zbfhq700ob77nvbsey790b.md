---
title: "Learning a system programming language: Rust"
datePublished: Fri Feb 18 2022 05:45:21 GMT+0000 (Coordinated Universal Time)
cuid: cl3zbfhq700ob77nvbsey790b
slug: learning-a-system-programming-language-rust
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654321456742/KHmxw54h9.jpeg
tags: rust, programming-languages

---

I have spent the past few weeks learning Rust through the official book: [The rust programming language](https://doc.rust-lang.org/stable/book/), and enjoyed [the experience](https://github.com/hanchiang/learn-rust). It is written by [Steve Klabnik](https://twitter.com/steveklabnik?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor), one of the former contributors of Rust.

I started learning Rust in 2019, left it, and only came back to it recently.

## Why systems programming?

I have been doing high-level web development for 3 years, and I wanted to work with technology at a lower and deeper level, which is systems programming

## Why Rust?

Besides being the [most-loved language](https://insights.stackoverflow.com/survey/2021) for consecutive years, Rust is designed to match the performance of C++, while providing memory safety, which is a safeguard for programmers against making such mistakes.

Interestingly, it is able to do so without the use of a garbage collector, through a concept known as [ownership](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html), and is one of the unique selling points of Rust.

## The Rust programming language book

As mentioned, the book is well written and covers everything you need to get started with Rust - data types, control structures, error handling, tests, packaging, concurrency, mini-projects.

Here are some of my favourite chapters:

[**4\. Understanding ownership**](https://doc.rust-lang.org/stable/book/ch04-00-understanding-ownership.html)

The concept of ownership is new and is Rust's unique selling point. The idea is that any heap-allocated data can only have 1 owner, so that freeing up the memory is straightforward.

This new way of approaching memory deallocation is new and takes time to get used to.

[**6\. Enums and Pattern Matching**](https://doc.rust-lang.org/book/ch06-00-enums.html)

Pattern matching using the *match* keyword enforce exhaustibility, i.e. Rust makes sure we handle every possible pattern, otherwise the program won't compile.

Together with pre-defined enums such as [Option](https://doc.rust-lang.org/std/option/), working with complex conditionals and variables that may or may not contain a value becomes a simple task.

[**9\. Error Handling**](https://doc.rust-lang.org/book/ch09-00-error-handling.html)

Building on enums and pattern matching, handling recoverable errors is a joy with the [Result](https://doc.rust-lang.org/std/result/enum.Result.html) enum, which contains the *Ok* variant if an operation succeeds, and the *Err* variant if it fails.

For uncoverable errors, simply use the [panic!](https://doc.rust-lang.org/std/macro.panic.html) macro.

[**15\. Smart pointers**](https://doc.rust-lang.org/stable/book/ch15-00-smart-pointers.html)

Smart pointers are pointers that behave like references, but with additional capabilities such as [dereferencing coercion](https://doc.rust-lang.org/std/ops/trait.Deref.html), [cleaning up resources](https://doc.rust-lang.org/std/ops/trait.Drop.html) after they are no longer needed.

Using a smart pointer such as [Rc](https://doc.rust-lang.org/std/rc/struct.Rc.html) allows a value to have multiple owners, and they are tracked and deallocated accordingly.

[**16\. Fearless concurrency**](https://doc.rust-lang.org/stable/book/ch16-00-concurrency.html)

Concurrent programming is important to any large-scale, high-load systems.

The concept of channels(similar to golang) allows threads to pass messages between one another, and mutex to allow threads access to shared resources.

[**12\. An I/O project: Building a command line program**](https://doc.rust-lang.org/stable/book/ch12-00-an-io-project.html) and [20\. Final project: building a multithreaded web server](https://doc.rust-lang.org/stable/book/ch20-00-final-project-a-web-server.html)

These 2 chapters walk through the process of building simple projects using the concepts learned so far and apply them in the context of proper software engineering practices such as [SOLID](https://en.wikipedia.org/wiki/SOLID), [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).

## Summary

I enjoyed the process of learning Rust and how memory safety is guaranteed through the ownership rule.

I may not use it for work any time soon, however this new way of thinking about memory has helped me become more aware of what goes underneath the hood when certain types of variables are created.

Also, I intend to continue exploring down the technology layers with systems programming and eventually use it as part of my work.