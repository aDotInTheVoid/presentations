+++
weight=10
+++
{{% section %}}
# Introduction

---

## Principle
- Quantum Computing without quantum physics
- Complex numbers and linear algebra (matrices/vectors)

---

## Rust

- *"A language empowering everyone
to build reliable and efficient software."* 
- Fast
- Strong, Static inferred types
- C like syntax
- Haskell like types
- Great docs/errors/threading/toolchain
- Great dependency management and linear algebra packages

---
```rust
fn main() {
    println!("Hello World!");
}
```
---
```rust
fn factorial_recursive(i: u64) -> u64 {
    match i {
        0 => 1,
        n => n * factorial(n-1)
    }
}

fn factorial_loop(i: u64) -> u64 {
    let mut acc = 1;
    for num in 2..=i {
        acc *= num;
    }
    acc
}

fn factorial_iter(i: u64) -> u64 {
    (1..=i).product()
}
```
---
## Into the code
- The code as presented will work
- I've ignored all module paths.
- The real code has more stuff to make it work for more than the minimal example.
- Any question's feel free to stop me.


{{% /section %}}
