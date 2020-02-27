+++
weight=20
+++
{{% section %}}
# Building Complex

---

```rust
#[derive(Debug, Clone, Copy)]
pub struct Complex {
   re: f32,
   im: f32,
}
```

---

```rust
impl Complex {
   pub fn new(re: f32, im: f32) -> Complex {
       Complex { re, im }
   }

   pub fn mod_arg(r: f32, theta: f32) -> Complex {
       Complex {
           re: r * theta.cos(),
           im: r * theta.sin(),
       }
   }
```

---

```rust
   pub fn exp_ix(x: f32) -> Complex {
       Complex::mod_arg(1.0, x)
   }

   pub fn from_re(re: f32) -> Complex {
       Self { re, im: 0.0 }
   }
```
---
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
```rust
   pub fn mag_square(&self) -> f32 {
       self.re.powi(2) + self.im.powi(2)
   }

   pub fn norm(&self) -> f32 {
       self.re.hypot(self.im)
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
impl AddAssign for Complex {
   fn add_assign(&mut self, other: Complex) {
       *self = *self + other;
   }
}
```
---
```rust
impl From<f32> for Complex{
   fn from(num: f32)-> Complex{
       Complex::from_re(num)
   }
}
```
{{% /section %}}
