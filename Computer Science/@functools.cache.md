#computer_theory #computer_theory #python 

You’re seeing **`@functools.cache`**, which is Python’s built-in **memoization**.

### What “cache” means here

- When you call a **pure** function (same inputs → same output), Python can **remember** the result the first time.
- Next time you call it **with the same arguments**, Python **skips recomputation** and instantly returns the saved result.

### How it works

- The decorator wraps your function and keeps a **dict-like store** that maps **arguments → result**.
    
- Keys are built from the function’s **positional and keyword arguments**, so **all arguments must be hashable** (e.g., ints, strings, tuples of hashables).
    
- First call with `(35)` computes and **fills the cache** (slow).
    
    Second call with `(35)` hits the cache (fast), which is why your second `%timeit` is much quicker.
    

### Why Fibonacci speeds up so much

Naive recursive Fibonacci recomputes the same subproblems many times (an exponential call tree).

With caching, each `fib(k)` is computed **once**, turning it into linear time.

### `cache` vs `lru_cache`

- `@functools.cache` (Python 3.9+): **unbounded** cache (equivalent to `@lru_cache(maxsize=None)`).
- `@functools.lru_cache(maxsize=...)`: caches up to `maxsize` most-recently-used results (evicts older ones), which **bounds memory**.

### Useful methods

```python
fibonacci_cached.cache_info()   # hits, misses, size, maxsize
fibonacci_cached.cache_clear()  # wipe the cache

```

### When to use it

- Functions that are:
    - **Pure** (no side effects, deterministic)
    - Called **often** with **repeated arguments**
    - **Expensive** to compute (I/O, heavy CPU, recursion, DP)

### When not to use it

- Arguments are **mutable/unhashable** (lists, dicts), or results depend on external state/time.
- The input space is huge (you’ll blow up memory).
- Functions with **side effects** (caching would skip those effects on repeated calls).

### Subtleties

- For instance methods, `self` is part of the key; different instances won’t share entries (unless you cache a `@staticmethod` or use a wrapper).
- Thread safety is basic; for heavily concurrent workloads consider your contention and memory.

In short: `@cache` trades memory for speed by remembering prior results, which is perfect for recursive DP-style functions like Fibonacci and many real-world repeated subcomputations.