+++
weight=40
+++
{{% section %}}
# Classical Registers
---
## New Type Idiom
```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct ClassicalRegister {
    pub bits: u8,
}
```