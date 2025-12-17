#python #computer_science 

Great question! The difference boils down to **what they produce** and **when they produce it**.

## Regular function (with `return`)

- **Produces a final result once**, then exits.
- **Eager**: builds all results in memory before you get them.
- Result can be **reused, indexed, iterated multiple times**.

```python
def squares_list(n):
    out = []
    for i in range(n):
        out.append(i*i)
    return out

vals = squares_list(5)         # [0, 1, 4, 9, 16]
vals[3]                        # 9 (random access ok)
for v in vals: pass            # reuse many times

```

## Generator function (with `yield`)

- **Produces a stream of values**, one at a time.
- **Lazy**: computes the next value only when asked.
- **Memory-efficient** and can represent **infinite** or very large sequences.
- **Single-use**: once you iterate it, it’s exhausted; no random access.

```python
def squares_gen(n):
    for i in range(n):
        yield i*i

g = squares_gen(5)             # generator object
list(g)                        # [0, 1, 4, 9, 16]
list(g)                        # [] (already consumed)

```

---

## When to use each

### Use a **generator** when…

1. **Data is large / streaming** (don’t want to hold everything in RAM)

```python
def read_numbers(path):
    with open(path) as f:
        for line in f:
            yield int(line)

total = sum(read_numbers("big.txt"))   # no giant list in memory

```

1. **You may stop early** (short-circuiting)

```python
def first_even(nums):
    return next((x for x in nums if x % 2 == 0), None)  # stops at first match

```

1. **Pipelines**: compose lazy steps

```python
def evens(nums):     # each step yields
    for x in nums:
        if x % 2 == 0:
            yield x

def squares(nums):
    for x in nums:
        yield x*x

result = sum(squares(evens(range(10_000_000))))

```

1. **Infinite / unbounded sequences**

```python
def naturals():
    i = 0
    while True:
        yield i
        i += 1

from itertools import islice
list(islice(naturals(), 5))  # [0,1,2,3,4]

```

### Use a **regular function (return a list/array)** when…

1. You **need random access / indexing / length** now

```python
def tokens(text):
    return [t.lower() for t in text.split()]  # list used many times

t = tokens("One Two Two")
t[1], len(t)

```

1. You’ll **iterate multiple times** over the same results

```python
vals = squares_list(10_000)
for v in vals: ...
for v in vals: ...   # no recompute; generator would need recreating

```

1. You need to **sort/shuffle/sample** the whole collection

```python
def top10(scores):
    return sorted([score(u) for u in users], reverse=True)[:10]

```

---

## Quick decision cheatsheet

- **One pass, large data, may stop early → generator.**
- **Need to reuse, index, or sort → regular function returning a list/array.**
- **Speed note:** For small data, a list can be slightly faster; for big data, generator saves memory and often time by avoiding allocation.

---

## Bonus: your reverse example, both ways

```python
# Generator (lazy, one pass)
def reverse_gen(x):
    for i in range(len(x)-1, -1, -1):
        yield x[i]

# Regular function (materialize)
def reverse_list(x):
    return list(reversed(x))   # or x[::-1]

```

Use `reverse_gen` when you’ll stream items once (maybe you’ll break early).

Use `reverse_list` when you’ll reuse the reversed data or need indexing.

---

Short answer: **it depends what you “store.”**

### 1) Store the **generator object**

```python
g = (i*i for i in range(5))   # generator expression

```

- `g` is **not** a list; it’s a **lazy, single-use iterator**.
- It computes values only when you iterate it.
- After one full pass, it’s **exhausted**; you can’t iterate again unless you recreate it.
- You **can’t index** it (`g[2]` fails) or get its length.

### 2) Store the **materialized results** (e.g., turn it into a list/tuple)

```python
vals = list(i*i for i in range(5))   # or [i*i for i in range(5)]

```

- Now `vals` is a **real list** just like what a regular function might return.
- You can **index, reuse, sort**, iterate multiple times, etc.
- At that moment you paid the full memory cost; the laziness benefit is gone.
- This is effectively the same outcome as using a regular function that returns a list.

### Quick comparison

```python
# generator stored
g = (i*i for i in range(3))
for x in g: print(x)        # 0 1 4
for x in g: print(x)        # prints nothing (already consumed)

# materialized
vals = list(i*i for i in range(3))
for x in vals: print(x)     # 0 1 4
for x in vals: print(x)     # 0 1 4 (reusable)
vals[1]                     # 1 (indexable)

```

### Subtle gotcha

Generators read from their **source at iteration time**. If the source changes before you consume the generator, you’ll see the **updated** data; a list would have captured a **snapshot** at creation.

**Rule of thumb**

- If you need **reuse/indexing** → materialize (list comprehension or `list(generator)`).
- If you’ll **consume once** and want to save memory → keep the **generator object**.