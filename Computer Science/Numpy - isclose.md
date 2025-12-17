#computer_theory #computer_science #math #python 

`np.isclose` checks element-wise whether two numbers/arrays are “close enough” within tolerances and returns a boolean array.

### Signature & rule

```python
np.isclose(a, b, rtol=1e-05, atol=1e-08, equal_nan=False)
```

An element `a[i]` is close to `b[i]` if:

```
abs(a - b) <= atol + rtol * abs(b)
```

- `rtol`: relative tolerance (scales with the magnitude of `b`)
    
- `atol`: absolute tolerance (useful near zero)
    
- `equal_nan=True`: treat `NaN` vs `NaN` as close
    

### Examples

```python
import numpy as np

np.isclose(0.30000000000000004, 0.3)
# True  (floating-point wobble)

np.isclose([1.0, 1.0e-9], [1.0 + 1e-7, 0.0], rtol=1e-6, atol=1e-8)
# array([ True, True ])

np.isclose([np.nan, 1.0], [np.nan, 1.0], equal_nan=True)
# array([ True, True ])

np.isclose([np.inf, -np.inf], [np.inf, -np.inf])
# array([ True, True ])
```

### Common uses

- Comparing floating-point results (unit tests, convergence checks).
    
- Deciding if something is “effectively zero”: `np.isclose(x, 0.0, atol=1e-12)`.
    
- Masking nearly equal values: `arr[np.isclose(arr, target, rtol, atol)]`.
    

### Tips & pitfalls

- For whole-array checks, use `np.allclose(a, b, ...)` (wraps `isclose` + `all`).
    
- Choose tolerances based on scale: near zero, rely more on `atol`.
    
- `isclose` broadcasts `a` and `b`; the result is boolean with the broadcasted shape.