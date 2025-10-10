#computer_theory #computer_science #math #python 

# `np.roll`

**What it does:** Circularly shifts (rotates) elements along an axis. Values that “fall off” one end reappear on the other.

**Signature:**

```python
np.roll(a, shift, axis=None)
```

- `shift`: how far to move.
    
    - Positive → toward higher indices.
        
    - Negative → toward lower indices.
        
- `axis`: which axis/axes to roll. If `None`, roll the flattened array.
    

**Examples:**

```python
a = np.array([0, 1, 2, 3, 4])
np.roll(a, 2)
# [3, 4, 0, 1, 2]

A = np.arange(9).reshape(3,3)
# [[0,1,2],
#  [3,4,5],
#  [6,7,8]]

np.roll(A, shift=1, axis=0)  # roll rows down by 1
# [[6,7,8],
#  [0,1,2],
#  [3,4,5]]

np.roll(A, shift=-1, axis=1) # roll columns left by 1
# [[1,2,0],
#  [4,5,3],
#  [7,8,6]]
```

**When useful (your context):** To compute neighbor sums with _periodic (toroidal) boundaries_, roll the grid in the 8 directions and add them—no explicit padding needed.

---

## Quick pitfalls & tips

- `np.pad` returns a **new** array (copy). Large pads cost memory/time.
    
- `np.roll` also returns a new array view/copy (effectively a copy in memory for many cases); it doesn’t just change a pointer.
    
- For performance in tight loops, minimize repeated large pads/rolls—vectorize where possible or precompute.