#computer_science #python 

Here’s a compact **pytest cheat sheet** you can keep open next to your editor.

# Install & Run

- Install: `pip install -U pytest` (extras: `pytest-cov`, `pytest-xdist`)
    
- Run all tests: `pytest`
    
- Run one file / test:
    
    `pytest tests/test_api.py`
    
    `pytest tests/test_api.py::TestAuth::test_login`
    
- Verbosity: `q` (quiet), `vv` (very verbose)
    
- Stop early: `x` or `-maxfail=1`
    
- Show prints/live logs: `s`
    
- Keyword select: `k "login and not slow"`
    
- Marker select: `m slow`
    

# Discovery Rules

- Files: `test_*.py` or `_test.py`
- Functions: `def test_...():`
- Classes: start with `Test` and methods start with `test_`

# Assertions (just use `assert`)

```python
assert a + b == 3
assert "x" in s
with pytest.raises(ValueError, match="bad"):
    bad()

```

Tip: Pytest shows rich diffs; no `unittest` asserts needed.

# Fixtures (setup/teardown)

```python
import pytest

@pytest.fixture
def db():
    conn = open_conn()
    yield conn
    conn.close()

def test_uses_fixture(db):
    assert db.is_open

```

- Share fixtures in `conftest.py` (auto-discovered, no import).
- Scope: `@pytest.fixture(scope="function" | "class" | "module" | "session")`

# Parametrization

```python
@pytest.mark.parametrize("a,b,expected", [(1,2,3),(0,0,0),(-1,1,0)])
def test_add(a,b,expected):
    assert a + b == expected

```

# Useful Built-in Fixtures

- `tmp_path` – temporary directory (pathlib)
    
- `monkeypatch` – set env vars/attrs
    
    ```python
    monkeypatch.setenv("MODE","test")
    monkeypatch.setattr(pkg.mod,"NOW", lambda: 0)
    
    ```
    
- `capsys` / `capfd` – capture stdout/stderr
    
- `request` – access current test context
    

# Markers (tags, skip, xfail)

```python
import pytest

@pytest.mark.slow
def test_big(): ...

@pytest.mark.skip(reason="flaky on Windows")
def test_skip(): ...

@pytest.mark.xfail(reason="bug #123", strict=True)
def test_known_bug(): ...

```

- Register custom markers in `pytest.ini` to silence warnings:

```
# pytest.ini
[pytest]
markers = slow: long-running tests

```

# CLI Power Moves

```bash
pytest -vv                                  # verbose names
pytest -k "foo and not bar"                 # expression filter
pytest -x --maxfail=1                       # fail fast
pytest -s                                   # show prints
pytest --durations=10                       # slowest tests
pytest --maxfail=1 -q                       # quiet, stop early

```

# Debugging

- Drop into debugger on failure: `pytest --pdb`
- Use breakpoints in code: `breakpoint()` and run `pytest -s`
- Nicer debugger: `pytest --pdbcls=IPython.terminal.debugger:TerminalPdb`

# Coverage (plugin)

```bash
pip install pytest-cov
pytest --cov=your_pkg --cov-report=term-missing

```

# Test Layout (typical)

```
your_project/
  your_pkg/
    __init__.py
    core.py
  tests/
    conftest.py
    test_core.py

```

# Config (recommended)

```
# pytest.ini or pyproject.toml [tool.pytest.ini_options]
[pytest]
addopts = -q
testpaths = tests
python_files = test_*.py

```

# Common Patterns

```python
# warnings
import warnings, pytest
def test_warns():
    with warnings.catch_warnings(record=True) as w:
        warnings.simplefilter("always")
        do_warn()
        assert any("deprecated" in str(x.message) for x in w)

# tmp files
def test_tmp(tmp_path):
    p = tmp_path/"out.txt"
    p.write_text("hi")
    assert p.read_text() == "hi"

# parameterized + ids
@pytest.mark.parametrize("x", [0,1,2], ids=lambda v: f"x={v}")
def test_ids(x): ...

```

# Parallel & Flaky

- Parallel runs: `pip install pytest-xdist` → `pytest -n auto`
- Retry flaky tests: `pip install pytest-rerunfailures` → `@pytest.mark.flaky(reruns=2)`

# Test Outcomes

- `.` pass, `F` fail (assert), `E` error (exception), `s` skipped, `x` xfailed

---

Want this as a 1-page printable PDF or an Obsidian-ready MD? I can format and export it.