#computer_science #python 

[[Doctest - Cheat Cheet]]
[[Doctest in Notebooks]]

**doctest** is a Python standard-library module that turns examples in your docstrings into runnable tests.

# What it does (in one line)

You write examples that look like an interactive Python session (`>>> ...`) inside docstrings; `doctest` runs them and checks the output matches exactly.

# Minimal example

```python
def add(a, b):
    """
    Return a + b.

    Examples
    --------
    >>> add(2, 3)
    5
    >>> add(-1, 1)
    0
    """
    return a + b

```

Run it:

```bash
python -m doctest -v your_module.py

```

or inside the module:

```python
if __name__ == "__main__":
    import doctest
    doctest.testmod()   # runs doctests in this module's docstrings

```

# Where you can put doctests

- Function/class/module **docstrings**
- External text files (e.g., `.rst`, `.txt`) using `python -m doctest -v file.rst`

# Common options / directives

Add after the expected output with a comment:

- `# doctest: +ELLIPSIS` — allow `...` to match anything
- `# doctest: +NORMALIZE_WHITESPACE` — ignore whitespace differences
- `# doctest: +SKIP` — skip this example

Example:

```python
>>> long_repr()
'Result(id=..., status=ok)'  # doctest: +ELLIPSIS

```

# Useful APIs

```python
import doctest
doctest.testmod(optionflags=doctest.ELLIPSIS | doctest.NORMALIZE_WHITESPACE)
doctest.testfile("README.rst", optionflags=doctest.ELLIPSIS, verbose=True)

```

# With pytest (super handy)

Run doctests alongside normal tests:

```bash
pytest --doctest-modules              # collect doctests from .py files
pytest --doctest-glob="*.rst"         # also pick up docs files

```

# Tips & gotchas

- Keep outputs **deterministic** (avoid randomness, current time, unordered sets/dicts in the printout).
- For floats, compare with tolerant text or use `ELLIPSIS` to trim representation noise.
- Doctests are **golden-master** style: they compare literal text. Great for examples and simple functions; not ideal for complex, stateful, or I/O-heavy behavior.
- Make examples small and focused—think “executable documentation”.

Want me to convert one of your function docstrings into clean doctests (or wire `pytest --doctest-modules` for your repo)?