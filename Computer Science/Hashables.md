#computer_theory #computer_theory #python 

In Python, **hashable** means:

- The object has a stable **hash value** (via `__hash__()` → an `int`) for its lifetime.
- It has a well-defined **equality** (`__eq__`) that’s consistent with the hash:
    - If `a == b`, then `hash(a) == hash(b)` must be true.
- Hashable objects can be used as **dict keys**, **set elements**, and as keys in memoization caches like `functools.cache`.

### Common hashable vs unhashable

- **Hashable (immutable):** `int`, `float`, `str`, `bytes`, `bool`, `tuple` _(only if all elements are hashable)_, `frozenset`, `None`.
    - e.g., `('user', 42)` is hashable; `('user', [42])` is **not** (list inside).
- **Unhashable (mutable):** `list`, `dict`, `set`, `bytearray`, most `numpy.ndarray`, `pandas` objects.
    - Their contents can change → hash wouldn’t stay stable.

### Custom classes

- Default: user-defined objects are hashable by identity (`id`) unless you define `__eq__`.
- If you override `__eq__` but **don’t** define `__hash__`, Python makes the class **unhashable** to avoid broken behavior.
- Using `@dataclass(frozen=True)` (or implementing `__hash__`) gives a safe, hashable value object.

### Quick checks

```python
hash(10)            # works → hashable
hash((1, 'a'))      # works → tuple of hashables
hash([1, 2])        # TypeError → list is unhashable

```

**Why you asked (memoization):** `@functools.cache` uses call arguments as dictionary keys, so all arguments must be **hashable**.