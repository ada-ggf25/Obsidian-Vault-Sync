#python #computer_science 

[[Regular Function Vs Generator Function]]

This example shows a **generator function**—a function that produces a sequence **lazily** using `yield` instead of building a list and `return`ing it.

```python
def reverse_list(x):
    for i in range(len(x) - 1, -1, -1):  # start at last index, down to 0
        yield x[i]                       # produce one item, pause here

```

- `yield` **emits one value** and _pauses_ the function, saving its state.
- On the next iteration, it resumes right after the last `yield`.
- When the loop ends, the generator raises `StopIteration` automatically.

Usage:

```python
x = [1, 2, 3, 4]
list(reverse_list(x))   # → [4, 3, 2, 1]

```

`reverse_list(x)` returns a **generator object** (a lazy, single-use iterable). Wrapping with `list(...)` **consumes** it and materializes the results.

Why this is nice:

- **Memory-efficient**: no intermediate reversed list is created.
- **Streamable**: you can iterate without holding everything in RAM.

FYI: for reversing sequences, Python already has an efficient iterator:

```python
list(reversed(x))  # same result

```