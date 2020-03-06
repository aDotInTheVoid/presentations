+++
weight=70
+++
{{% section %}}
# Binary Gates
---
## Imports
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
## Type definition
```rust
#[derive(Debug, Clone, PartialEq)]
pub struct BinaryGate {
    mat: Matrix,
}
```
---
## Methods
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
## Controlled Not (CNOT)
```text
|A> ---●--- |A>      
       |  
|B> ---⊕--- |A ⊕ B>
```
- Top is passed through
- Bottom is NOT when top is TRUE (XOR)
- $\begin{bmatrix}1&0&0&0\cr 0&1&0&0 \cr 0&0&0&1 \cr 0&0&1&0 \end{bmatrix}$
- $\ket{00} \rarr \ket{00}$, $\ket{01} \rarr \ket{01}$, $\ket{10} \rarr \ket{11}$, $\ket{11} \rarr \ket{10}$
---
## CNOT Constructor
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
