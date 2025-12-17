#computer_science #python 

Here are the clean, practical ways to run **doctest inside a Jupyter notebook**.

# 1) Run doctests for everything defined in the notebook

Put examples in your function/class **docstrings** (with `>>>`), then:

```python
def add(a, b):
    """
    Return a + b.

    >>> add(2, 3)
    5
    """
    return a + b

```

Run doctest (tests the current “module”, i.e., the notebook kernel):

```python
import doctest
doctest.testmod(verbose=True)   # runs doctests for all objects defined so far

```

# 2) Run doctests for a single object

Useful while iterating:

```python
import doctest

def mul(a, b):
    """
    >>> mul(2, 4)
    8
    """
    return a * b

doctest.run_docstring_examples(mul, globals(), verbose=True)

```

# 3) Run doctest via pytest from a cell (nice reporting)

If your code lives in `.py` files (recommended), you can run doctests with pytest:

```python
# pip install pytest
import pytest, sys
sys.exit(pytest.main(["--doctest-modules", "-q"]))  # discovers doctests in .py files

```

You can target specific files or patterns:

```python
sys.exit(pytest.main(["--doctest-glob=*.rst", "docs/", "-q"]))

```

# 4) Test notebook _documents_ (Markdown examples)

`doctest` doesn’t read Markdown cells. If you want to validate notebooks themselves:

- **nbmake** (re-executes notebooks under pytest):
    
    ```bash
    pip install nbmake
    
    ```
    
    In a cell:
    
    ```python
    import pytest, sys
    sys.exit(pytest.main(["--nbmake", "notebooks/my_nb.ipynb", "-q"]))
    
    ```
    
- **nbval** (compares stored outputs):
    
    ```bash
    pip install nbval
    
    ```
    
    In a cell:
    
    ```python
    import pytest, sys
    sys.exit(pytest.main(["--nbval", "notebooks/my_nb.ipynb", "-q"]))
    
    ```
    

# 5) Handy options inside notebooks

Use option flags when variability exists (float repr, whitespace, etc.):

```python
import doctest
doctest.testmod(optionflags=doctest.ELLIPSIS | doctest.NORMALIZE_WHITESPACE,
                verbose=True)

```

Inline directives in examples:

```python
def info(x):
    """
    >>> info("abc")
    'abc ...'  # doctest: +ELLIPSIS
    """
    return f"{x} and more"

```

# Quick checklist

- Put `>>>` examples in docstrings of functions/classes defined in cells.
    
- Run `doctest.testmod()` to test everything defined so far, or
    
    `doctest.run_docstring_examples(obj, globals())` for one object.
    
- For larger projects, keep logic in `.py` modules and run
    
    `pytest --doctest-modules` from a cell for better output.