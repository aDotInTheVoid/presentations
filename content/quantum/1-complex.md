+++
weight=20
+++
{{% section %}}
# Complex
---
```rust
use approx::{AbsDiffEq, RelativeEq};
use num_traits::identities::{One, Zero};

use std::ops::{
    Add, AddAssign, Div, DivAssign, Mul, MulAssign, Neg, Sub,
    SubAssign,
};
```
---
```rust
/// A complex number
///
/// ```rust
/// # use toy_quant::complex::Complex;
/// let x = Complex::new(1.0, 0.0);
/// assert_eq!(x, Complex::one());
/// ```
#[derive(Debug, Clone, Copy, PartialEq)]
pub struct Complex {
    re: f32,
    im: f32,
}
```
---
```rust
impl Complex {
    /// Create a complex number from a real and imaginary part
    pub fn new(re: f32, im: f32) -> Self {
        Self { re, im }
    }
```
---
```rust
    /// Create a complex number from a modulus and argument
    pub fn mod_arg(r: f32, theta: f32) -> Self {
        Self {
            re: r * theta.cos(),
            im: r * theta.sin(),
        }
    }
```
---
```rust
    /// Create a complex number e^ix, equivalent to mod_arg(1, x)
    pub fn exp_ix(x: f32) -> Complex {
        Complex::mod_arg(1.0, x)
    }
```
---
```rust
    /// Create a complex number with a real part and no imaginary part
    pub fn from_re(re: f32) -> Complex {
        Self { re, im: 0.0 }
    }
```
---
```rust
    /// The complex number 0 + 0i
    pub fn zero() -> Complex {
        Complex::new(0.0, 0.0)
    }

    /// The complex number 1 + 0i
    pub fn one() -> Complex {
        Complex::new(1.0, 0.0)
    }
```
---
```rust
    /// √-1
    pub fn i() -> Complex {
        Complex::new(0.0, 1.0)
    }
```
---
```rust
    /// |x|²
    pub fn mag_square(self) -> f32 {
        self.re.powi(2) + self.im.powi(2)
    }
```
---
```rust
    /// |x|
    pub fn norm(self) -> f32 {
        self.re.hypot(self.im)
    }
```
---
```rust
    /// The complex conjugate. Re(x) - i Im (x). a-bi
    pub fn conj(self) -> Self {
        Self {
            re: self.re,
            im: -self.im,
        }
    }
}
```
---
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
```
---
```rust
impl Sub<Complex> for Complex {
    type Output = Self;
    fn sub(self, other: Self) -> Self {
        Self {
            re: self.re - other.re,
            im: self.im - other.im,
        }
    }
}
```
---
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
```
---
```rust
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
```rust
impl Mul<Complex> for f32 {
    type Output = Complex;
    fn mul(self, other: Complex) -> Complex {
        other * self
    }
}
```
---
```rust
impl From<f32> for Complex {
    fn from(num: f32) -> Complex {
        Complex::from_re(num)
    }
}
```
---
```rust
impl From<u8> for Complex {
    fn from(num: u8) -> Complex {
        Complex::from_re(num.into())
    }
}
```
---
```rust
impl Div<Complex> for Complex {
    type Output = Self;
    // We have tests for this, and clippy freaks out
    // when I have a addition in a division function.
    #[allow(clippy::suspicious_arithmetic_impl)]
    fn div(self, other: Self) -> Self {
        let a = self.re;
        let b = self.im;
        let c = other.re;
        let d = other.im;
        // a+bi   (a+bi)(c-di)   (ac+bd) + i(bc-ad)
        // ---- = ------------ = ------------------
        // c+di   (c+di)(c-di)     c^2 + d^2
        let denom = c.powi(2) + d.powi(2);
        let rep = a * c + b * d;
        let imp = b * c - a * d;
        Self {
            re: rep / denom,
            im: imp / denom,
        }
    }
}
```
---
```rust
impl AddAssign for Complex {
    fn add_assign(&mut self, other: Complex) {
        *self = *self + other;
    }
}
```
---
```rust
impl SubAssign for Complex {
    fn sub_assign(&mut self, other: Complex) {
        *self = *self - other;
    }
}
```
---
```rust
impl MulAssign for Complex {
    fn mul_assign(&mut self, other: Complex) {
        *self = *self * other;
    }
}
```
---
```rust
impl DivAssign for Complex {
    fn div_assign(&mut self, other: Complex) {
        *self = *self / other;
    }
}
```
---
```rust
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
```rust
impl Zero for Complex {
    fn zero() -> Self {
        0.0.into()
    }
    fn is_zero(&self) -> bool {
        self == &Self::zero()
    }
}
```
---
```rust
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