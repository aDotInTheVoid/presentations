+++
weight=90
+++
{{% section %}}
# Conclusion
---
## Still to do
- Sparse Matrix/Vector Optimization
- Ternary gates
- Apply gates to register of larger sizes
- More circuits
- Repeated sampling helpers
- Use standard `complex` implementation
- Noise simulation
---
## Sparse Matrix/Vector Optimization
- Most matrix/vector entries are $0$.
- More efficient matrix/vector storage and multiplication methods exist for these.
- Could allow faster speed/more intensive simulations
---
### Ternary gates
- Gates with 3 inputs
- Eg: Controlled swap
- $\begin{bmatrix}1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \cr0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \cr0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \cr0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \cr0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \cr0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \cr0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \cr0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \end{bmatrix}$
---
## Apply gates to register of larger sizes
- Currently a unary gate can only be applied to a qubit, and a binary gate can only be applied to a register of 2 qubits
- This means a lot of fiddling is needed to move data into the right form
- Also limits the circuits that can be build
- Would be better if the data lived in 1 register for the entire life cycle and any gate could be run on the register as long as it had enough qubits
---
## More circuits
- Only one circuit example
- Should try more to see how much it can do
---
## Repeated sampling helpers
- Currently registers provide no way to repeadidly sample
- This should be added
---
## Use standard `complex` implementation
- I've got my own implementation of complex numbers
- Using a standard 3'rd party one would make the code simpler and more interoperable
---
## Noise simulation
- Real quantum computers have random errors called noise
- It would be nice to simulate this
---
## The code
- `https://github.com/aDotInTheVoid/toy-quant`
{{% /section %}}
