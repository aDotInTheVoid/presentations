+++
weight=60
+++
{{% section %}}
# Unary Gates
---
```rust
//! Gates that map a qubit to a qubit

use std::f32::consts::FRAC_1_SQRT_2;

use crate::complex::Complex;
use crate::qubit::Qubit;

use approx::assert_relative_eq;
use nalgebra;

use num_traits::identities::{one, zero};

type Matrix = nalgebra::Matrix2<Complex>;
```
---
```rust
#[derive(Debug, Clone, PartialEq)]
pub struct UnaryGate {
    mat: Matrix,
}
```
---
```rust
impl UnaryGate {
    pub fn new(mat: Matrix) -> Self {
        let x = mat * mat.transpose().map(|x| x.conj());
        assert_relative_eq!(x, Matrix::identity());
        Self { mat }
    }
    pub fn run(&self, q: Qubit) -> Qubit {
        Qubit {
            inner: self.mat * q.inner,
        }
    }
}
```
---
```rust
pub fn h() -> UnaryGate {
    UnaryGate::new(
        Matrix::new(
            one(), one(), 
            one(), -one::<Complex>(), 
        )
        .map(|x| x * FRAC_1_SQRT_2),
    )
}
```
{{% /section %}}
