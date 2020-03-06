+++
weight=60
+++
{{% section %}}
# Unary Gates
---
## Imports
```rust
use std::f32::consts::FRAC_1_SQRT_2;

use crate::complex::Complex;
use crate::qubit::Qubit;

use approx::assert_relative_eq;
use nalgebra;

use num_traits::identities::{one, zero};

type Matrix = nalgebra::Matrix2<Complex>;
```
---
## Type definition
```rust
#[derive(Debug, Clone, PartialEq)]
pub struct UnaryGate {
    mat: Matrix,
}
```
---
## Matrix Correctness

- $|\alpha|^2+|\beta|^2 = 1$
- $\bold{M} \begin{bmatrix}\alpha\cr \beta \end{bmatrix}=\begin{bmatrix}\alpha^{\prime}\cr \beta^{\prime} \end{bmatrix}$
- $|\alpha^{\prime}|^2+|\beta^{\prime}|^2 = 1$
- What does this say about $\bold{M}$
- $\bold{M}^\dagger\bold{M}=\bold{I}$ where $\bold{X}^\dagger=(\bold{M}^\intercal)^*$
- Proof ~~left as an excessive~~ outside the scope
- Bonus: geometric interpretation

---
## Methods
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
## Hadamard gate
- $\bold{H}=\frac{1}{\sqrt{2}}\begin{bmatrix}1&1\cr 1&-1 \end{bmatrix}$
- $\ket{0} \rarr \frac{\ket{0}+\ket{1}}{2} = \ket{+}$
- $\ket{1} \rarr \frac{\ket{0}-\ket{1}}{2} = \ket{-}$
- $\ket{+} \rarr \ket{0}$
- $\ket{-} \rarr \ket{1}$
- $\bold{H}^2 = \bold{I}$

---
## Hadamard gate constructor
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
