#python #computer_theory #computer_science

[[Numba Vs Cython]]

Numba is a **just-in-time (JIT) compiler** for Python that speeds up numeric code by compiling selected functions to fast machine code (via LLVM) **at runtime**.

### Why/when to use it

Use Numba when you have **tight loops** or math-heavy “kernels” in pure Python/NumPy that are slow. It shines with:

- Numerical loops, elementwise ops, simulations
- Array math where vectorizing in NumPy is awkward
- Parallel loops and simple reductions
- GPU kernels (CUDA)

### Core ideas (super short)

- Decorate functions with `@njit` (no-Python mode). Numba **type-infers** arguments and compiles to native code; the GIL is released inside compiled code.
- Works best with **NumPy arrays**, scalars, and simple Python types.
- Optional **parallelization**: `@njit(parallel=True)` + `prange`.
- **Vectorized ufuncs**: `@vectorize`, `@guvectorize`.
- **GPU**: `from numba import cuda`; write `@cuda.jit` kernels.

### Minimal examples

```python
from numba import njit, prange
import numpy as np

@njit
def saxpy(a, x, y):
    out = np.empty_like(x)
    for i in range(x.size):
        out[i] = a*x[i] + y[i]
    return out

@njit(parallel=True, fastmath=True)
def row_sums(A):
    n, m = A.shape
    s = np.empty(n, dtype=A.dtype)
    for i in prange(n):
        tot = 0.0
        for j in range(m):
            tot += A[i, j]
        s[i] = tot
    return s

```

### Common switches & tips

- `@njit(cache=True)` → stores compiled machine code on disk for faster cold starts.
- `fastmath=True` → allows unsafe FP optimizations (faster, but tiny numeric differences).
- Provide **signatures** to avoid first-call compile overhead (optional).
- Use **typed containers** (`numba.typed.List`) if you need dynamic lists.

### Limitations (gotchas)

- Only a **subset of Python** is supported: no arbitrary objects, dynamic features, or most of pandas.
- Best with fixed-dtype NumPy arrays; Python lists/dicts are limited.
- If Numba can’t compile in nopython mode, it may **fall back** to object mode (slow). Prefer `@njit` which **errors** instead of silently falling back.
- Random: `np.random` works but with some constraints.
- Works on **CPython**; not PyPy. GPU requires an NVIDIA GPU + CUDA toolkit.

### Numba vs alternatives

- **NumPy**: great if you can vectorize; Numba covers the “hard-to-vectorize loops”.
- **Cython**: ahead-of-time, more control; Numba is quicker to try—just add a decorator.
- **PyTorch/JAX**: better for autodiff/ML; Numba is general-purpose numeric JIT.

**One-liner:** Numba makes numerical Python nearly C-fast by JIT-compiling your hot functions, with optional parallelism and CUDA support—usually by just adding a decorator.

---

Great tool—but not a silver bullet. Here’s why you **don’t always use Numba**:

1. **Warm-up/compile overhead**
    
    First call JIT-compiles. For short scripts, tiny arrays, or functions called once, the compile time can dominate.
    
2. **Limited Python subset**
    
    Numba excels with **NumPy arrays & scalars**. Many features are unsupported or awkward: most of pandas, Python objects, dicts/sets, strings, dynamic typing, recursion, generators, etc. If it can’t compile in nopython mode, it may fail (or be slow).
    
3. **You might already be calling optimized C**
    
    Vectorized **NumPy/BLAS** often matches or beats Numba. Re-implementing a `np.dot` in Numba will be slower.
    
4. **I/O-bound work won’t speed up**
    
    Numba doesn’t help with disk/network waits or Python-level orchestration.
    
5. **Debugging & errors**
    
    Compile-time type errors are less friendly. Debugging optimized kernels is trickier than plain Python.
    
6. **Parallelism overhead**
    
    `parallel=True`/`prange` helps for big workloads, but adds overhead; small problems can get slower.
    
7. **Deployment/packaging constraints**
    
    Requires compatible `llvmlite/numba`. Cold-start JIT at runtime can be undesirable for CLIs, servers, or notebooks (unless you use `cache=True`).
    
8. **Recompiles with changing types/shapes**
    
    Different dtypes or signatures trigger new compilations; lots of variability can hurt.
    
9. **Better tools for some jobs**
    
    - Need AOT & tight C/C++ interop → **Cython/pybind11**
    - Need autodiff/accelerators/graph transforms → **JAX/PyTorch**
    - Need GPU kernels with control → **CUDA** directly or CuPy.

### Use Numba when

- You’ve **profiled** a hot, numeric **loop** that’s hard to vectorize.
- Operates on **large arrays / many iterations**, called **repeatedly**.
- Uses **NumPy dtypes** and simple control flow.

### Avoid or reconsider when

- Workload is small/one-off, I/O-bound, or already vectorized.
- Heavy pandas/string/object logic.
- Packaging/latency constraints make JIT inconvenient.

Rule of thumb: **Profile → vectorize first → Numba for the stubborn loops.**