+++
weight=30
+++
{{% section %}}
# Qubits
---
## Imports
```rust
use approx::assert_relative_eq;

use nalgebra::Vector2;

use rand::prelude::*;
use rand::rngs::SmallRng;

use crate::complex::Complex;
```
---
## Type definitions
- $ \ket{\psi} = \alpha\ket{0} + \beta\ket{1} $
- $ \alpha,\ \beta\in\  ℂ $
- $ |\alpha|^2 + |\beta|^2 = 1$
- $\ket{0}$ and $\ket{1}$ can be thought as orthogonal basis vectors
- Stored as $\begin{bmatrix}\alpha\cr \beta \end{bmatrix}$
```rust
#[derive(Debug, Clone, PartialEq)]
pub struct Qubit {
    pub(crate) inner: Vector2<Complex>,
}
```
---
## Constructors
```rust
impl Qubit {
    pub fn new(p_0: Complex, p_1: Complex) -> Self {
        assert_relative_eq!(1.0, p_0.mag_square() + p_1.mag_square());
        Qubit {
            inner: Vector2::new(p_0, p_1),
        }
    }

    pub fn zero() -> Self {
        Self::new(Complex::one(), Complex::zero())
    }

    pub fn one() -> Self {
        Self::new(Complex::zero(), Complex::one())
    }
```
---
## Sampling
```rust
    pub fn sample_is_zero(&self) -> bool {
        SmallRng::from_entropy()
            .gen_bool(self.inner.index(0).mag_square().into())
    }

    pub fn sample(&self) -> f32 {
        if self.sample_is_zero() {
            0.0
        } else {
            1.0
        }
    }
}
```
{{% /section %}}