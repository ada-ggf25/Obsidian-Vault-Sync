#computer_science #python 

Here are the **best ways to use pytest in Jupyter notebooks**—pick the one that fits your workflow.

# 1) Run your project’s tests from a cell

```python
import pytest, sys

# basic (quiet)
sys.exit(pytest.main(["-q"]))

# examples:
# sys.exit(pytest.main(["tests/test_api.py::TestAuth::test_login", "-vv", "-s"]))
# sys.exit(pytest.main(["-k", "vector and not slow", "-x", "-vv"]))

```

Tip: add `"-s"` to see `print()` output; add `"--maxfail=1"` to stop early.

# 2) Write & run tests **inline** in the notebook (with ipytest)

```python
# pip install ipytest
import ipytest, pytest
ipytest.autoconfig()   # uses the current kernel

%%ipytest -q
def add(a, b): return a + b

def test_add():
    assert add(2, 3) == 5

```

- Put multiple `%%ipytest` cells as you iterate.
- Use flags like `q`, `vv`, `k "pattern"`, `s`.

# 3) Test the **notebooks themselves** (CI-friendly)

- **nbmake** (re-executes notebooks with pytest):
    
    ```bash
    pip install nbmake
    pytest --nbmake notebooks/  # or a single .ipynb
    
    ```
    
- **nbval** (validates outputs; doesn’t re-run by default):
    
    ```bash
    pip install nbval
    pytest --nbval my_notebook.ipynb
    # or re-execute with lax mode:
    pytest --nbval-lax my_notebook.ipynb
    
    ```
    

# 4) Recommended structure (smoothest dev loop)

- Put real code in `.py` modules (import them in notebooks).
- Keep tests in `tests/` and call pytest from a cell (option 1) or terminal.
- Use `%load_ext autoreload; %autoreload 2` to pick up code changes without restarting the kernel.

# Debugging inside notebooks

- Drop breakpoints in your code and run tests with `s`:
    
    ```python
    # in code under test
    breakpoint()
    
    ```
    
    Then in the notebook cell:
    
    ```python
    import pytest, sys
    sys.exit(pytest.main(["-q", "-s", "--maxfail=1"]))
    
    ```
    
- Or run `%pdb on` to auto-enter pdb on exceptions you trigger in cells (outside pytest).
    

If you tell me your repo layout (paths for `tests/` and modules), I’ll give you a copy-paste cell tailored to it.