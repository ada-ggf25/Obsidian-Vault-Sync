#computer_science #python 

This slide contrasts **list comprehensions** and **generator expressions**.

```python
numbers = list(range(3))             # [0, 1, 2]

doubled_list = [2*i for i in numbers]   # LIST COMPREHENSION
doubled_ge   = (2*i for i in numbers)   # GENERATOR EXPRESSION

```

### What happens

- **List comprehension** (`[...]`):
    - **Eager**: computes _all_ results immediately.
    - Stores them in memory as a Python list.
    - `doubled_list` prints as `[0, 2, 4]`.
- **Generator expression** (`(...)`):
    - **Lazy**: computes items **on demand**, one by one.
    - Holds no full list in memory—just the state of iteration.
    - Printing it shows `<generator object ...>` because it’s an **iterator**, not the data.

### How to use the generator

Iterate or consume it:

```python
list(doubled_ge)      # [0, 2, 4]
sum(2*i for i in numbers)   # 6
for x in doubled_ge:
    print(x)          # 0, 2, 4

```

_(Note: once a generator is exhausted, you can’t iterate it again unless you recreate it.)_

### When to use which

- Use **list comprehension** when you **need the whole collection** multiple times.
- Use a **generator expression** when you only **iterate once** or want to **save memory** (especially for large/streaming data).

Rule of thumb: `[...]` = materialize now; `(...)` = produce on demand.

---

You’ve got it mostly right. The key is **laziness** and **single-pass**:

- **Generator expression** `(...)`
    - **Computes on demand**, yields one item at a time.
    - **Single use**: once you iterate it, it’s exhausted.
    - Great when you’ll **consume once** (e.g., `sum`, `any`, `for` loop) and don’t need random access or reuse.
- **List comprehension** `[...]`
    - **Builds everything now** and stores it.
    - **Reusable & indexable**; you can iterate many times.
    - Costs memory up front.

### When to prefer each (with examples)

### Use a **generator** when you’ll consume once (memory-friendly)

```python
# Streaming a huge file: don’t build a giant list
total = sum(int(line) for line in open("big.txt"))

# Short-circuiting: stops early as soon as condition is met
has_neg = any(x < 0 for x in data)

# Pipelines: keep it lazy through steps
result = sum( (x*x for x in data if x % 2 == 0) )

```

### Use a **list** when you need reuse, indexing, or multiple passes

```python
# Reuse multiple times
squares = [x*x for x in data]
print(len(squares))
print(squares[10:20])
for v in squares: ...
for v in squares: ...   # iterate again without recomputing

# You’ll sort, shuffle, or random access
evens_sorted = sorted([x for x in data if x % 2 == 0])

```

### Subtle performance rule of thumb

- If you’ll **consume the whole sequence once**, a generator avoids allocating the list and is often better for **large** data.
- If you’ll **use the results multiple times** or need **random access**, pay the one-time cost of a list.
- For **small** data, either is fine; list comps are often a tiny bit faster to build than creating a generator and then immediately materializing it.

### Common patterns

```python
# Good generator patterns
first_big = next((x for x in data if x > 1_000_000), None)  # stops early
product  = math.prod(x for x in data if x)                  # consume once

# Good list patterns
tokens = [tok.lower() for tok in text.split()]              # reused later
top10  = sorted([score(u) for u in users], reverse=True)[:10]

```

**Bottom line:**

- **One pass, no reuse → generator.**
- **Reuse, indexing, or multiple passes → list.**