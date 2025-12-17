#computer_science #python 

Here’s a tight **doctest cheat sheet** you can keep by your editor.

# What is it?

Turn examples in **docstrings** (that look like REPL sessions) into tests.

```python
def add(a, b):
    """
    Return a + b.

    >>> add(2, 3)
    5
    """
    return a + b

```

---

# How to run

**CLI (quick):**

```bash
python -m doctest -v your_module.py
python -m doctest -v docs/guide.rst

```

**From code (module self-test):**

```python
if __name__ == "__main__":
    import doctest
    doctest.testmod()  # runs doctests in this module

```

**Test a docs file:**

```python
import doctest
doctest.testfile("README.rst", optionflags=doctest.ELLIPSIS, verbose=True)

```

**With pytest (recommended):**

```bash
pytest --doctest-modules                 # pick up .py docstrings
pytest --doctest-glob="*.rst"            # also test docs files

```

---

# Writing examples

**Basic input/output**

```python
>>> "py" * 2
'pypy'

```

**Multiple lines**

```python
>>> s = []
>>> s.append(1); s.append(2)
>>> s
[1, 2]

```

**Exceptions (match type and message)**

```python
>>> int("x")
Traceback (most recent call last):
...
ValueError: invalid literal for int() with base 10: 'x'

```

**Floating point (be explicit or use flags)**

```python
>>> round(0.1 + 0.2, 1)
0.3

```

---

# Directives (inline controls)

Add after the expected output as a comment:

- `# doctest: +ELLIPSIS` — allow `...` to match anything
- `# doctest: +NORMALIZE_WHITESPACE` — ignore whitespace differences
- `# doctest: +SKIP` — skip this example
- `# doctest: +IGNORE_EXCEPTION_DETAIL` — ignore varying traceback details

Example:

```python
>>> repr(obj)
'Widget(id=..., status=ok)'  # doctest: +ELLIPSIS

```

---

# Useful option flags (programmatic)

Combine with `|` and pass via `optionflags=`:

- `doctest.ELLIPSIS`
    
- `doctest.NORMALIZE_WHITESPACE`
    
- `doctest.IGNORE_EXCEPTION_DETAIL`
    
- `doctest.REPORT_ONLY_FIRST_FAILURE`
    
- Diff styles for failures: `REPORT_NDIFF`, `REPORT_UDIFF`, `REPORT_CDIFF`
    
- Strictness toggles:
    
    `doctest.DONT_ACCEPT_TRUE_FOR_1`, `doctest.DONT_ACCEPT_BLANKLINE`
    
- `doctest.FAIL_FAST`
    

Example:

```python
doctest.testmod(optionflags=doctest.ELLIPSIS | doctest.NORMALIZE_WHITESPACE)

```

---

# Common patterns

**Make examples deterministic**

```python
>>> import random; random.seed(0)
>>> random.randint(1, 3)
1

```

**Control printing for dict/set order (Python 3.7+ is stable, but be safe)**

```python
>>> sorted({"b":2,"a":1}.items())
[('a', 1), ('b', 2)]

```

**Hide setup noise**

```python
>>> _ = cache.clear()     # assign to _ to avoid output

```

**Long outputs (use ellipsis)**

```python
>>> long_str()
'prefix ... suffix'  # doctest: +ELLIPSIS

```

---

# Typical project setup

- Put runnable examples in your functions/classes docstrings.
    
- Add a quick self-check:
    
    ```python
    if __name__ == "__main__":
        import doctest; doctest.testmod()
    
    ```
    
- Wire CI to run:
    
    ```bash
    python -m doctest -v your_pkg/*.py
    pytest --doctest-modules
    
    ```
    

---

# Quick checklist

- Use `>>>` and exact outputs.
- Keep examples short, deterministic, and copy-pastable in a REPL.
- Use directives (`+ELLIPSIS`, `+NORMALIZE_WHITESPACE`) when formatting varies.
- Prefer pytest integration for unified reporting.

If you want, I can turn this into a one-page printable PDF or an Obsidian-ready `.md`.