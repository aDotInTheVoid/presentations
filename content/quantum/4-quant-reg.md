+++
weight=50
+++
{{% section %}}
# Quantum Registers
---
## Imports
```rust
use std::convert::TryInto;

use rand;

use super::classical::ClassicalRegister;
use crate::complex::Complex;
use crate::qubit::Qubit;
use nalgebra::allocator::Allocator;
use nalgebra::default_allocator::DefaultAllocator;
use nalgebra::dimension::DimName;
use nalgebra::dimension::*;
use nalgebra::Vector4;
use nalgebra::VectorN;
```
---
## Type Definition
```rust
#[derive(Clone, PartialEq, Debug)]
pub struct QuantumRegister<N: DimName>
where
    DefaultAllocator: Allocator<Complex, N>,
{
    qubits: VectorN<Complex, N>,
}
```
---
## Conversion methods
```rust
impl<N: nalgebra::dimension::DimName> QuantumRegister<N>
where
    DefaultAllocator: Allocator<Complex, N>,
{
    pub fn from_vector(qubits: VectorN<Complex, N>) -> Self {
        Self { qubits }
    }

    pub fn into_vector(self) -> VectorN<Complex, N> {
        self.qubits
    }
```
---
## Collapse logic
```rust
    fn collapse_with_target(&self, target: f32) -> ClassicalRegister {
        let target = target % 1.0;
        let mut current = 0.0;
        let mut reserve: Option<u8> = None; // Handle floating point errors
        for (bits, im_prob) in self.qubits.iter().enumerate() {
            let prob = im_prob.mag_square();
            current += prob;
            if current > target {
                return ClassicalRegister {
                    // bits from iter is usize, need to cast to u8, might fail
                    bits: bits.try_into().unwrap() 
                };
            } else if prob != 0.0 {
                reserve = Some(bits.try_into().unwrap());
            }
        }
        ClassicalRegister {
            bits: reserve.unwrap(),
        }
    }
```
---
## Public collapse API
```rust
    // This is separate from collapse_with_target so the tests
    // can try funky numbers to check for floating point errors.
    pub fn collapse(&self) -> ClassicalRegister {
        self.collapse_with_target(rand::random::<f32>())
    }
}
```
---
## Merging Qubits
```rust
impl QuantumRegister<U4> {
    pub fn from_2_qubits(qa: Qubit, qb: Qubit) -> Self {
        let (qa_0, qa_1) = (qa.inner.index(0), qa.inner.index(1));
        let (qb_0, qb_1) = (qb.inner.index(0), qb.inner.index(1));
        let ket_00 = *qa_0 * *qb_0;
        let ket_01 = *qa_0 * *qb_1;
        let ket_10 = *qa_1 * *qb_0;
        let ket_11 = *qa_1 * *qb_1;
        Self {
            qubits: Vector4::new(ket_00, ket_01, ket_10, ket_11),
        }
    }
}
```
{{% /section %}}