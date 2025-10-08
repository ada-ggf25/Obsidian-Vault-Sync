#computer_theory #computer_science #math #python 

Great question. What you’re seeing is about **data sharing** (views) vs **new storage** (copies) in NumPy—not exactly “shallow vs deep copy” in the plain-Python sense.

### What your cells show

- `mat1 = np.resize(mat, (3, 2))`
- `mat2 = np.resize(mat, (3, 9))`
- `mat3 = np.reshape(mat, (3, 2))`

Then:

- `mat1.base is mat -> False`
- `mat3.base is mat -> True`

**Interpretation**

- `arr.base` tells you whether an array **shares the same data buffer** with another array.
    - `True` (or `arr.base is mat`) → it’s a **view** (no new data allocated; metadata-only change).
    - `False` (often `arr.base is None` or not `mat`) → it’s a **copy** (new data buffer).

So in your case:

- `np.reshape(mat, (3,2))` → **view when possible** → shares data with `mat` → `mat3.base is mat` is `True`.
- `np.resize(...)` (the **function**) → **always returns a new array**. If the new size is larger, it **repeats** the data to fill; if smaller, it truncates. It **does not** share storage → `mat1.base is mat` is `False` (same for `mat2`).

### Does this mean shallow vs deep copy?

Roughly:

- A **view** is like a _shallow_ copy of the array **metadata** only; it points to the **same underlying data**.
- A **copy** duplicates the **data buffer** (what we usually mean by _deep_ copy in NumPy context).

But in NumPy we usually say **view vs copy**:

|Operation|Shares data?|Notes|
|---|---|---|
|**Basic slicing** (e.g., `a[1:3, ::-1]`)|Yes (view)|Zero-copy; changes reflect in original.|
|**`reshape`**|View if possible; else copy|Your example produced a view. Non-contiguous inputs may force a copy.|
|**`transpose` / `.T`**|Yes (view)|Changes strides; no data move.|
|**Fancy indexing** (`a[[...]]`, masks)|No (copy)|Always allocates new array.|
|**`ravel`**|View if possible; else copy|`flatten` always copies.|
|**`astype`**|Copy (usually)|Unless `dtype` unchanged and `copy=False`, still often copies.|
|**`np.copy`, `arr.copy()`**|Copy|New buffer.|
|**`np.resize` (function)**|Copy|New array; repeats/truncates to fit shape.|
|**`ndarray.resize` (method)**|In-place resize of the **original**|May reallocate and invalidate views; use with care.|

### Extra tip

To check ownership quickly:

```python
a.flags['OWNDATA']   # True if a owns its buffer

```

and

```python
b.base is a          # True if b is a view into a

```

So: different NumPy functions indeed have different copy semantics. Your outputs mean **`reshape` gave you a view**, while **`resize` gave you new arrays (copies)**.