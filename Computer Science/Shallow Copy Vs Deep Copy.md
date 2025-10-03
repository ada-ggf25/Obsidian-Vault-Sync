#python #computer_science 

Great question! In Python, “copy” usually means a **shallow copy**, while a “deep copy” is something different done by `copy.deepcopy`. Here’s the full picture.

# The core idea

- **Shallow copy**: makes a **new container** (new list/dict/set/…),  
    but its **elements are the same objects** as in the original (just new references).
    
- **Deep copy**: makes a **new container** and **recursively copies** (almost) everything inside,  
    so nested objects become **independent clones**.
    

Think: _shallow = new box, same items; deep = new box, duplicated items_.

---

## Quick demo

```python
import copy

a = [[1, 2], [3, 4]]  # nested lists

b = copy.copy(a)      # shallow copy (same as a[:] or list(a))
c = copy.deepcopy(a)  # deep copy

b is a          # False (new outer list)
b[0] is a[0]    # True  (inner list is the same object)

c is a          # False
c[0] is a[0]    # False (inner list cloned)
```

Mutating a nested element shows the difference:

```python
b[0].append(99)
a   # [[1, 2, 99], [3, 4]]  <-- changed (because inner list is shared)

c[0].append('X')
a   # [[1, 2, 99], [3, 4]]  <-- unchanged (c is fully independent)
```

---

## How to make a shallow copy

- Lists: `new = old[:]`, `list(old)`, or `copy.copy(old)`
    
- Dicts: `new = old.copy()`, `dict(old)`, or `copy.copy(old)`
    
- Sets: `new = old.copy()`, `set(old)`, or `copy.copy(old)`
    
- Tuples: copying just returns the **same tuple** (tuples are immutable), but note: if a tuple holds mutables, those mutables are still shared.
    

All of the above copy **only one level**; nested objects are **not** cloned.

---

## How `deepcopy` behaves

- `copy.deepcopy(obj)` recursively clones most built-ins and user objects.
    
- It uses an internal **memo** dictionary to:
    
    - avoid copying the same object multiple times,
        
    - handle **cycles** (self-referencing structures),
        
    - preserve aliasing (if `a[0] is a[1]` in the original, then `b[0] is b[1]` in the deep copy).
        
- You can customize copying on your classes by defining `__copy__(self)` and `__deepcopy__(self, memo)`.
    

**Limitations:** Some objects shouldn’t or can’t be deep-copied (e.g., file handles, sockets, locks, GPU handles). In such cases, implement custom logic or avoid deep copying.

---

## Immutables vs mutables

- Immutables (ints, strings, tuples of immutables, `frozenset`) are effectively “shared safely”. Shallow vs deep doesn’t matter for them.
    
- Mutables (lists, dicts, sets, custom objects) **do** matter: shallow copy shares them; deep copy clones them.
    

---

## Performance & memory

- **Shallow copy** is fast and memory-light (just a new outer container + references).
    
- **Deep copy** can be expensive (time and memory grow with the size and depth of the structure).
    

**Rule of thumb:** prefer shallow copy unless you truly need independent nested state.

---

## When to use which

- Use **shallow copy** when:
    
    - You only need a new outer container (e.g., you’ll replace whole sub-objects, not mutate them), or
        
    - The elements are immutable (so sharing is safe).
        
- Use **deep copy** when:
    
    - You will mutate **nested** structures and want to **isolate** changes from the original, or
        
    - You’re cloning a complex config/tree/graph that will be modified independently.
        

**Hybrid pattern:** copy shallowly, then deep-copy only the parts you’ll mutate:

```python
import copy
y = old_config.copy()
y["model"] = copy.deepcopy(old_config["model"])  # only this subtree needs isolation
```

---

## Special notes for NumPy/Pandas

- **NumPy**: `arr.copy()` copies the underlying buffer (deep w.r.t. the data), but if `dtype=object` it behaves like Python containers (watch out).
    
- **Pandas**: `df.copy(deep=True)` clones data; `deep=False` shares data (metadata is still copied).
    

---

### TL;DR

- **Shallow copy** = new container, same inner objects.
    
- **Deep copy** = new container, recursively cloned inner objects.
    
- Choose shallow for speed when safe; deep only when you truly need full independence of nested state.