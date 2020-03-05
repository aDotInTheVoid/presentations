+++
weight=70
+++
{{% section %}}
# Binary Gates
---
```rust
use crate::complex::Complex;
use crate::registers::quantum::QuantumRegister;

use approx::assert_relative_eq;
use nalgebra::U4;

type Matrix = nalgebra::Matrix4<Complex>;
type MatrixU8 = nalgebra::Matrix4<u8>;
type Register2 = QuantumRegister<U4>;
```
---
```rust
#[derive(Debug, Clone, PartialEq)]
pub struct BinaryGate {
    mat: Matrix,
}
```
---
```rust
impl BinaryGate {
    pub fn new(mat: Matrix) -> Self {
        let x = mat * mat.transpose().map(|x| x.conj());
        assert_relative_eq!(x, Matrix::identity());
        Self { mat }
    }

    pub fn new_u8(mat: MatrixU8) -> Self {
        Self::new(mat.map(Complex::from))
    }

    pub fn apply(&self, qubits: Register2) -> Register2 {
        Register2::from_vector(self.mat * qubits.into_vector())
    }
}
```
---
```rust
pub fn cnot() -> BinaryGate {
    BinaryGate::new_u8(MatrixU8::new(
        1, 0, 0, 0, 
        0, 1, 0, 0,
        0, 0, 0, 1, 
        0, 0, 1, 0,
    ))
}
```


{{% /section %}}
