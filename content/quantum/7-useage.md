+++
weight=80
+++
{{% section %}}
# Usage Example
---
```rust
use nalgebra::dimension::*;
use toy_quant::{
    gates::{binary::gates::cnot, unitary::gates::h},
    qubit::Qubit,
    registers::quantum::QuantumRegister,
};
```
---
```rust
fn entangle_qubits(
    ket_a: Qubit,
    ket_b: Qubit,
) -> QuantumRegister<U4> {
    let ket_a = h().run(ket_a);
    let merged = QuantumRegister::from_2_qubits(ket_a, ket_b);
    cnot().apply(merged)
}
```
---
```rust
fn eval_qubits(ket_a: Qubit, ket_b: Qubit) {
    println!("∣{}{}⟩ becomes", ket_a.sample(), ket_b.sample());
    let mut states = [0, 0, 0, 0];
    let reg = entangle_qubits(ket_a, ket_b);
    for _ in 0..1000 {
        states[reg.collapse().bits as usize] += 1;
    }
    for (idx, val) in states.iter().enumerate() {
        println!("∣{:02b}⟩ * {}", idx, *val as f32 / 1000.0)
    }
    println!("");
}
```
---
```rust
fn main() {
    eval_qubits(Qubit::zero(), Qubit::zero());
    eval_qubits(Qubit::zero(), Qubit::one());
    eval_qubits(Qubit::one(), Qubit::zero());
    eval_qubits(Qubit::one(), Qubit::one());
}
```
---
|In|Out|
|--|--|
|$\ket{00}$|$\frac{\ket{00}+\ket{11}}{\sqrt{2}}$|
|$\ket{01}$|$\frac{\ket{01}+\ket{10}}{\sqrt{2}}$|
|$\ket{10}$|$\frac{\ket{00}-\ket{11}}{\sqrt{2}}$|
|$\ket{11}$|$\frac{\ket{01}-\ket{10}}{\sqrt{2}}$|

{{% /section %}}
