+++
weight=10
+++
{{% section %}}
# Introduction

---

## Quantum Computing without quantum physics

---

## Qubits


- $ \ket{\psi} = \alpha\ket{0} + \beta\ket{1} $
- $ \alpha,\ \beta\in\  ℂ $
- $ |\alpha|^2 + |\beta|^2 = 1$
- $\ket{0}$ and $\ket{1}$ can be thought as orthogonal basis vectors
- $|\alpha|^2$ is the probability of observing $\ket{0}$

---

## Rust

- *"A language empowering everyone
to build reliable and efficient software."* 
- Fast
- Strong, Static inferred types
- C like syntax
- Haskell like types
- Great docs/errors/threading/toolchain

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



{{% /section %}}
