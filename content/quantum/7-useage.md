+++
weight=80
+++
{{% section %}}
# Usage: Bell states
---
## Imports
```rust
use nalgebra::dimension::U4;
use toy_quant::{
    gates::{binary::gates::cnot, unitary::gates::h},
    qubit::Qubit,
    registers::quantum::QuantumRegister,
};
```
---
## The circuit
<img src="https://upload.wikimedia.org/wikipedia/commons/f/fc/The_Hadamard-CNOT_transform_on_the_zero-state.png" style="filter: invert(100%);"/>

---
## The circuit code
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
## Helper function
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
## Main function
```rust
fn main() {
    eval_qubits(Qubit::zero(), Qubit::zero());
    eval_qubits(Qubit::zero(), Qubit::one());
    eval_qubits(Qubit::one(), Qubit::zero());
    eval_qubits(Qubit::one(), Qubit::one());
}
```
---
## What should happen
|In|Out|
|--|--|
|$\ket{00}$|$\frac{\ket{00}+\ket{11}}{\sqrt{2}}$|
|$\ket{01}$|$\frac{\ket{01}+\ket{10}}{\sqrt{2}}$|
|$\ket{10}$|$\frac{\ket{00}-\ket{11}}{\sqrt{2}}$|
|$\ket{11}$|$\frac{\ket{01}-\ket{10}}{\sqrt{2}}$|

---
# The output
```txt
∣00⟩ becomes    ∣01⟩ becomes    ∣10⟩ becomes    ∣11⟩ becomes                        
∣00⟩ * 0.506    ∣00⟩ * 0        ∣00⟩ * 0.492    ∣00⟩ * 0                          
∣01⟩ * 0        ∣01⟩ * 0.481    ∣01⟩ * 0        ∣01⟩ * 0.498                  
∣10⟩ * 0        ∣10⟩ * 0.519    ∣10⟩ * 0        ∣10⟩ * 0.502                  
∣11⟩ * 0.494    ∣11⟩ * 0        ∣11⟩ * 0.508    ∣11⟩ * 0                          
```
---
## Formatted
|                 | $\ket{00}$ in | $\ket{01}$ in | $\ket{10}$ in | $\ket{10}$ in   |
|-----------------|---------------|---------------|---------------|-----------------|
| $\ket{00}$ out  | $0.506$       | $0    $       | $0    $       | $0.492$         |                  
| $\ket{01}$ out  | $0    $       | $0.481$       | $0.498$       | $0    $         |               
| $\ket{10}$ out  | $0    $       | $0.519$       | $0.502$       | $0    $         |               
| $\ket{11}$ out  | $0.494$       | $0    $       | $0    $       | $0.508$         |                   

{{% /section %}}
