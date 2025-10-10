#computer_science #python 

[[Pytest - Cheat Cheet]]
[[Pytest in Notebooks]]

Here’s a crisp, practical intro to **pytest** and how to use it right away.

# What is pytest?

A fast, flexible Python testing framework. It:

- auto-discovers tests (no boilerplate classes needed),
- lets you write plain `assert` statements,
- gives powerful **fixtures** for setup/teardown,
- has rich plugins (e.g., coverage, flaky, xdist).

---

# Install

```bash
pip install -U pytest          # optional: pytest-cov pytest-xdist

```

---

# 60-second start

**1) Create a file** `test_math.py`:

```python
def add(a, b):
    return a + b

def test_add_simple():
    assert add(2, 3) == 5

```

**2) Run tests**

```bash
pytest            # discovers tests named test_*.py or *_test.py

```

**3) Read output**

- ✅ dots for passing tests
- F for failures (with helpful diffs)
- E for errors (tracebacks)

---

# Naming & discovery

- Files: `test_*.py` or `_test.py`
- Functions: `def test_something():`
- Classes (optional): start with `Test` and methods start with `test_`.

---

# Assertions (just use `assert`)

```python
def test_list():
    xs = [1, 2, 3]
    assert 2 in xs
    assert xs[:2] == [1, 2]

```

Pytest shows diffs for failing asserts (no `unittest` methods needed).

---

# Fixtures (setup/teardown done right)

```python
import pytest

@pytest.fixture
def db_conn():
    # setup
    conn = {"open": True}
    yield conn
    # teardown
    conn["open"] = False

def test_uses_fixture(db_conn):
    assert db_conn["open"] is True

```

- Put shared fixtures in `conftest.py` (no imports needed across tests).

---

# Parametrization (test many cases at once)

```python
import pytest

@pytest.mark.parametrize("a,b,expected", [
    (2, 3, 5),
    (0, 0, 0),
    (-1, 1, 0),
])
def test_add(a, b, expected):
    assert a + b == expected

```

---

# Useful built-in fixtures

- `tmp_path` – temporary filesystem path
- `monkeypatch` – patch env vars/attributes/functions
- `capsys` – capture stdout/stderr

```python
def test_print(capsys):
    print("hello")
    out, err = capsys.readouterr()
    assert "hello" in out

```

---

# Handy CLI flags (you’ll use these a lot)

```bash
pytest -q                  # quiet
pytest -vv                 # verbose (shows each test name)
pytest -k "add and not slow"  # run tests matching an expression
pytest tests/test_api.py::TestAuth::test_login  # run a single test
pytest -x --maxfail=1      # stop after first failure
pytest -s                  # don’t capture stdout/stderr (show prints)
pytest --pdb               # drop into pdb on failure
pytest -n auto             # run tests in parallel (needs pytest-xdist)

```

---

# Markers (tag/skip/xfail)

```python
import pytest

@pytest.mark.slow
def test_big():
    ...

@pytest.mark.skip(reason="broken on Windows")
def test_skip_me():
    ...

@pytest.mark.xfail(reason="bug #123")
def test_known_bug():
    assert 0

```

Run selected markers: `pytest -m slow`

---

# Coverage (with plugin)

```bash
pip install pytest-cov
pytest --cov=your_pkg --cov-report=term-missing

```

---

# Typical project layout

```
your_project/
  your_pkg/
    __init__.py
    core.py
  tests/
    conftest.py
    test_core.py

```

---

# Using with pdb (ties to your earlier question)

- Break anywhere: `breakpoint()` in code, then run `pytest -q -s`.
- Auto-drop on failure: `pytest --pdb` (use `l`, `p var`, `n`, `c`, etc.).
- Nicer debugger UI (optional): `pytest --pdbcls=IPython.terminal.debugger:TerminalPdb`

---

# In notebooks

- Put logic in `.py` files, test with `pytest` from terminal.
    
- Or run inside a cell:
    
    ```python
    import pytest, sys
    sys.exit(pytest.main(["-q"]))
    
    ```
    

If you tell me what you’re testing (e.g., a data pipeline, API client, or NumPy-heavy code), I’ll sketch a minimal, idiomatic test suite with fixtures and parametrization tailored to it.