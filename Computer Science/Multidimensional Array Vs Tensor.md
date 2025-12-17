#computer_science #python #math #computer_theory

Great question! Short answer: in most programming contexts, a “tensor” is just an n-dimensional array—but the word carries extra math/ML meaning and framework-specific features.

### The core idea

- **Multidimensional array:** A data structure for values indexed by multiple indices (shape, strides, memory layout). Example: a NumPy `ndarray` with shape `(32, 64, 3)`.
- **Tensor (math):** An abstract multilinear object that generalizes scalars (0-D), vectors (1-D), matrices (2-D) to **n-D**. Any n-D array of numbers _represents_ a tensor once you fix a basis.

### In practice (programming/ML frameworks)

- **NumPy**: `ndarray` = n-D array. It’s a tensor in the mathematical sense, but NumPy doesn’t call it “Tensor”.
- **PyTorch / TensorFlow / JAX**: `Tensor` objects = n-D arrays **plus**:
    - a **device** (CPU/GPU/TPU),
        
    - a **dtype**,
        
    - optional **autograd** info (tracks ops for backprop),
        
    - sometimes constraints (e.g., immutable vs. mutable semantics).
        
        These features are why ML folks say “tensor” rather than just “array”.
        

### Terminology gotchas

- **Rank**:
    - In tensor lingo, _rank_ = number of axes (aka order). A matrix has rank 2.
    - In linear algebra, _matrix rank_ = dimension of the column/row space. Different concept!
- **Contiguity/layout**: Arrays emphasize memory layout (row-/column-major, strides). Tensors have the same concerns; frameworks expose `.is_contiguous()`, strides, etc.

### Quick mapping

- 0-D: scalar → tensor/array with shape `()`
- 1-D: vector → shape `(n,)`
- 2-D: matrix → shape `(m, n)`
- n-D: higher-order tensor/array → shape `(d1, d2, …, dk)`

**Bottom line:** Every tensor can be stored as a multidimensional array; “tensor” is the broader mathematical term and, in ML code, usually means “nd-array with extra framework semantics (device, autograd, etc.).”