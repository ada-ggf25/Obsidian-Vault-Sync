#computer_theory #computer_science #math #python 

Here’s what each part is doing:

```python
d = np.arange(9).reshape(3,3)
print(d)

```

- `np.arange(9)` → array `[0,1,2,3,4,5,6,7,8]`.
    
- `.reshape(3,3)` → make it 3×3:
    
    ```
    [[0 1 2]
     [3 4 5]
     [6 7 8]]
    
    ```
    

```python
print(d[::-1, ::-1])

```

- Slicing uses `[rows, cols]`.
    
- `::-1` means “take everything with step −1” (reverse).
    
- First `::-1` flips the **rows** (top↔bottom); second `::-1` flips the **columns** (left↔right).
    
- Net effect: flip on both axes (equivalently a 180° rotation):
    
    ```
    [[8 7 6]
     [5 4 3]
     [2 1 0]]
    
    ```
    

```python
print(d[(slice(None, None, -1), slice(None, None, -1))])

```

- Same operation, just written with explicit `slice(start, stop, step)` objects:
    - `slice(None, None, -1)` ≡ `::-1`.
- Produces the same flipped/rotated array.

Notes:

- These are **basic slices**, so NumPy returns a **view** (no data copied).
- Useful variants:
    - `d[::-1, :]` → flip vertically.
    - `d[:, ::-1]` → flip horizontally.