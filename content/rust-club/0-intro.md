+++
weight = 10
+++
{{% section %}}

# Intro 

---


## Why Rust

- Safe
- Concurrent
- Fast

--- 

## Safe

- All pointers valid
- All data typed
- Automatic Deallocation
- No segfault
- No memory leeks

---

## Concurrent

- Concurrency without fear
- The type system detects concurrent access to data and requires synchronisation
- No data races
- Good synchronization types (atomics, mutex) available
- Async functions are first class citizens

---

## Fast

- As fast a c
- Optimizing compiler based on LLVM
- Static dispatch
- No runtime
- Zero cost abstractions
- Auto SIMD

---

## Pragmatism

- Allows `unsafe` for raw memory access
- `std` has safe abstractions over unsafe code (`Vec`, `HashMap`)
- FFI
- *Amazing* compiler errors

---

## Language features

- `c` based syntax
- Expression based language
- Haskell inspired types

---

## Well known users

- Firefox
- Gnome
- Dropbox
- Microsoft
- Amazon
- Google
- Discord

--- 

## Installation

- Linux: `rustup.rs`, requires `build-essential`
- Macos: `rustup.rs`, requires xcode 
- Windows: `rustup.rs`, requires visual studio (c++)

--- 

## Toolchain

- Rustup (installer)
- Cargo (build system)
- Rustc (compiller)
- Crates.io (package registry)

---


## Hello world
```rust
fn main(){
    println!("Hello World")
}
```
```bash
$ rustc hello.rs
$ ./hello # or .\main.exe on windows
Hello World
```

--- 

## Hello cargo

```bash
$ cargo new hello_cargo
$ cd hello_cargo
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/hello_cargo`
Hello, world!
$ cargo check
   Checking hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
$ cargo build --release
$ cargo run --release
```
{{% /section %}}
