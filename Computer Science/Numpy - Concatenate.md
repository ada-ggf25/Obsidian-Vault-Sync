#computer_theory #computer_science #math #python 

You’re horizontally stitching two 2-D NumPy arrays together.

```python
a1 = np.array([[1,2,3],
               [4,5,6]])        # shape (2, 3)

a2 = np.array([[7,8,9],
               [10,11,12]])     # shape (2, 3)

a3 = np.concatenate((a1, a2), axis=1)
print(a3)

```

- `axis=1` means **concatenate columns** (side-by-side).
- The arrays must have the **same number of rows** (here 2).
- Result shape becomes `(2, 3 + 3) = (2, 6)`:

```
[[ 1  2  3  7  8  9]
 [ 4  5  6 10 11 12]]

```

Notes:

- `np.concatenate` **creates a new array** (copies data).
- Shortcuts: `np.hstack((a1,a2))` is equivalent here; `axis=0` (or `np.vstack`) would stack them vertically (one under the other).