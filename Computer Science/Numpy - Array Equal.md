#computer_theory #computer_science #math #python 

`np.array_equal(a, b, *, equal_nan=False)` checks if two arrays have the **same shape** and **exactly equal elements** (no tolerances). It returns a single `True/False`.

- **Shape must match** exactly (no broadcasting).
    
- **Exact equality**: integers/bools/strings must match; floats must be exactly equal bit-wise except for the `equal_nan` option.
    
- **NaNs**: by default, `NaN != NaN`, so arrays with NaNs in the same places are **not** equal unless you pass `equal_nan=True`.
    
- **Dtypes don’t have to match** if values compare equal under NumPy’s comparison rules (e.g., `int` vs `float` with the same values can still be `True`).
    

### Examples

```python
import numpy as np

np.array_equal(np.array([1,2,3]), np.array([1,2,3]))          # True
np.array_equal(np.array([1,2,3]), np.array([1,2,3,4]))        # False (shape)
np.array_equal(np.array([1]), np.array([1.0]))                # True (values equal)
np.array_equal(np.array([np.nan]), np.array([np.nan]))        # False
np.array_equal(np.array([np.nan]), np.array([np.nan]), equal_nan=True)  # True
```

### Related functions (when to use what)

- `np.array_equiv(a, b)`: like `array_equal` **but allows broadcasting**.
    
- `np.allclose(a, b, rtol, atol)`: for **floats with tolerance** (also allows broadcasting).
    
- `numpy.testing.assert_array_equal(a, b)`: test helper that raises with a helpful diff instead of returning `False`.