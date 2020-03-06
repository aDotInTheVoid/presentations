+++
weight=20
+++
{{% section %}}
# Complex Numbers
---
## Type definition
```rust
use approx::{AbsDiffEq, RelativeEq};
use num_traits::identities::{One, Zero};

use std::ops::{Add, AddAssign, Mul, MulAssign, Neg};

#[derive(Debug, Clone, Copy, PartialEq)]
pub struct Complex {
    re: f32,
    im: f32,
}
```
---
## Constructors
```rust
impl Complex { 
    pub fn new(re: f32, im: f32) -> Self {
        Self { re, im }
    }
    
    pub fn from_re(re: f32) -> Complex {
        Self { re, im: 0.0 }
    }
```
---
## Fixed values
```rust 
    pub fn zero() -> Complex {
        Complex::new(0.0, 0.0)
    }

    pub fn one() -> Complex {
        Complex::new(1.0, 0.0)
    }

    pub fn i() -> Complex {
        Complex::new(0.0, 1.0)
    }
```
---
## Other methods
```rust
    pub fn mag_square(self) -> f32 {
        self.re.powi(2) + self.im.powi(2)
    }

    pub fn conj(self) -> Self {
        Self {
            re: self.re,
            im: -self.im,
        }
    }
}
```
---
## Addition
```rust
impl Add<Complex> for Complex {
    type Output = Self;
    fn add(self, other: Self) -> Self {
        Self {
            re: self.re + other.re,
            im: self.im + other.im,
        }
    }
}

impl AddAssign for Complex {
    fn add_assign(&mut self, other: Complex) {
        *self = *self + other;
    }
}
```
---
## Multiplication
```rust
impl Mul<Complex> for Complex {
    type Output = Self;
    fn mul(self, other: Self) -> Self {
        Self {
            re: self.re * other.re - self.im * other.im,
            im: self.re * other.im + self.im * other.re,
        }
    }
}

impl Mul<f32> for Complex {
    type Output = Self;
    fn mul(self, other: f32) -> Self {
        Self {
            re: self.re * other,
            im: self.im * other,
        }
    }
}
```
---
## Conversion
```rust
impl From<f32> for Complex {
    fn from(num: f32) -> Complex {
        Complex::from_re(num)
    }
}

impl From<u8> for Complex {
    fn from(num: u8) -> Complex {
        Complex::from_re(num.into())
    }
}
```
---
## Miscellaneous traits
```rust
impl MulAssign for Complex {
    fn mul_assign(&mut self, other: Complex) {
        *self = *self * other;
    }
}

impl Neg for Complex {
    type Output = Complex;

    fn neg(self) -> Complex {
        Self {
            re: -self.re,
            im: -self.im,
        }
    }
}
```
---
## Number traits
```rust
impl Zero for Complex {
    fn zero() -> Self {
        0.0.into()
    }
    fn is_zero(&self) -> bool {
        self == &Self::zero()
    }
}

impl One for Complex {
    fn one() -> Self {
        (1.0).into()
    }
    fn is_one(&self) -> bool {
        self == &Self::one()
    }
}
```
---
## Approximate Comparison I
```rust
impl AbsDiffEq for Complex {

    type Epsilon = <f32 as AbsDiffEq>::Epsilon;

    fn default_epsilon() -> Self::Epsilon {
        f32::default_epsilon()
    }

    fn abs_diff_eq(
        &self,
        other: &Self,
        epsilon: Self::Epsilon,
    ) -> bool {
        f32::abs_diff_eq(&self.re, &other.re, epsilon)
            && f32::abs_diff_eq(&self.im, &other.im, epsilon)
    }
}
```
---
## Approximate Comparison II
```rust
impl RelativeEq for Complex {
    fn default_max_relative() -> <f32 as AbsDiffEq>::Epsilon {
        f32::default_max_relative()
    }

    fn relative_eq(
        &self,
        other: &Self,
        epsilon: Self::Epsilon,
        max_relative: Self::Epsilon,
    ) -> bool {
        f32::relative_eq(&self.re, &other.re, epsilon, max_relative)
            && f32::relative_eq(
                &self.im,
                &other.im,
                epsilon,
                max_relative,
            )
    }
}
```
{{% /section %}}