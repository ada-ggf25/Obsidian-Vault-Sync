#computer_theory #computer_science #math #python 

Here’s the practical cheat-sheet for `np.random`—with modern best practices.

# TL;DR

- Use the **new API**: `rng = np.random.default_rng(seed)` → then call methods on `rng`.
    
- Avoid old globals like `np.random.rand`, `np.random.randint` in new code (they still work, but are legacy).
    
- All samplers are **vectorized**: pass `size=...` to draw many at once.
    

# Create a generator (recommended)

```python
import numpy as np
rng = np.random.default_rng(42)     # reproducible
# different algorithms: PCG64 (default), Philox, SFC64, etc.
```

# Common samplers (on a Generator)

```python
rng.random(size)                    # U[0,1) floats
rng.integers(low, high=None, size=None, endpoint=False)
# high is EXCLUSIVE by default; set endpoint=True to include it

rng.normal(loc=0.0, scale=1.0, size=None)        # Gaussian
rng.uniform(low=0.0, high=1.0, size=None)        # Uniform
rng.binomial(n, p, size=None)
rng.poisson(lam, size=None)
rng.exponential(scale=1.0, size=None)
rng.beta(a, b, size=None)
rng.gamma(shape, scale=1.0, size=None)
rng.multivariate_normal(mean, cov, size=None, check_valid='warn')

rng.choice(a, size=None, replace=True, p=None)   # sample from 1D array or range(a)
rng.permutation(x)                               # return permuted copy
rng.shuffle(x)                                   # in-place shuffle along first axis
```

### Examples

```python
# 1) 10 floats in [0,1)
u = rng.random(10)

# 2) 5 integers in {0,1,2,3,4}
k = rng.integers(0, 5, 5)

# 3) 100x100 normal(0,1)
Z = rng.normal(0, 1, (100, 100))

# 4) Weighted draw without replacement
cards = np.arange(52)
draw = rng.choice(cards, size=5, replace=False, p=None)

# 5) Permute rows of a matrix
A = np.arange(12).reshape(4,3)
A_perm = rng.permutation(A)  # new array
rng.shuffle(A)               # in-place shuffle of rows
```

# Reproducibility tips

- Set the seed **once** where randomness starts:
    
    ```python
    rng = np.random.default_rng(123)
    ```
    
- **Avoid global state** (e.g., `np.random.seed(...)` and `np.random.rand(...)`) in libraries; pass `rng` down to functions instead:
    
    ```python
    def simulate(nsteps, rng=None):
        rng = np.random.default_rng() if rng is None else rng
        X = rng.random((16,16)) > 0.3
        ...
    ```
    

# Legacy API (still around, not preferred)

- `np.random.rand`, `np.random.randn`, `np.random.randint`, `np.random.choice`, etc. use a hidden global generator (`RandomState`). Fine for quick scripts; avoid in libraries/tests that need deterministic behavior.
    

# Gotchas

- `integers(high)` vs `randint(high)`: default **exclusive** upper bound for `integers`; `randint` is also exclusive at `high`.
    
- `choice(p=...)`: probabilities must sum to 1 (within floating tolerance).
    
- `shuffle` is **in-place**; `permutation` returns a **new** array.
    
- For very large draws, sample **once** with a `size` tuple instead of looping in Python.
    

If you show me a specific sampling pattern from your automata (e.g., random initial states or random rule noise), I’ll translate it to the `Generator` style.