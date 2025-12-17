#computer_theory #computer_science #math #python 

Here’s the quick, practical rundown.

# `np.pad`

**What it does:** Adds a border (padding) around an array.

**Core idea:** You specify _how much_ to pad on each edge and _how to fill_ that padding.

**Common signature:**

```python
np.pad(a, pad_width, mode='constant', constant_values=0)
```

- `pad_width`: how many values to add on each side.
    
    - 1D: `k` or `(left, right)`
        
    - 2D: `((top, bottom), (left, right))`, etc.
        
- `mode`: how to fill the pad. Most used:
    
    - `'constant'` → fill with `constant_values` (default 0)
        
    - `'edge'` → repeat edge values
        
    - `'reflect'` / `'symmetric'` → mirror the array
        
    - `'wrap'` → wrap around (toroidal)
        

**Examples:**

```python
import numpy as np

a = np.array([1, 2, 3])
np.pad(a, 2, mode='constant', constant_values=-1)
# [-1, -1, 1, 2, 3, -1, -1]

A = np.arange(4).reshape(2,2)
# [[0,1],
#  [2,3]]

np.pad(A, ((1,1),(2,2)), mode='edge')
# Pads 1 row top/bottom and 2 cols left/right by repeating edges
```

**When useful (your context):** For cellular automata, padding with `'wrap'` or `'constant'` can make neighbor calculations easier, especially at boundaries.