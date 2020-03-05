+++
weight=40
+++
{{% section %}}
# Classical Registers
---
```rust
use std::iter::*;

#[derive(Debug, Clone, PartialEq, Eq)]
pub struct ClassicalRegister {
    pub bits: u8,
}
```
---
```rust
impl FromIterator<bool> for ClassicalRegister {
    fn from_iter<I: IntoIterator<Item = bool>>(iter: I) -> Self {
        let mut bits = 0;
        for (n_bits, bit) in iter.into_iter().enumerate() {
            assert!(
                n_bits < 8,
                "Got {} bits, but the register can only hold 8",
                n_bits + 1
            );
            bits |= (bit as u8) << n_bits;
        }
        Self { bits }
    }
}
```
---
```rust
impl From<u8> for ClassicalRegister {
    fn from(bits: u8) -> Self {
        Self { bits }
    }
}
```
---
```rust
impl ClassicalRegister {
    pub fn index(&self, index: u8) -> bool {
        assert!(index < 8);
        ((self.bits >> index) & 1) == 1
    }

    pub fn set(&mut self, index: u8, val: bool) {
        if val {
            self.bits |= 1 << index;
        } else {
            self.bits &= !(1 << index);
        }
    }
}
```
{{% /section %}}