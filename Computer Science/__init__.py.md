#python #computer_science 

Great question—`__init__.py` is about **packages** and **package APIs**.

# What it is

- Any folder on `PYTHONPATH` with an `__init__.py` is treated as a **package**.
- The file is **executed once** when the package is first imported: `import mypkg` runs `mypkg/__init__.py`.

> Since Python 3.3 (PEP 420), a folder without **init**.py can still be a package (implicit namespace package). So today, the file is optional—but still very useful.

---

# Why/when to use it

## 1) To define a clean package API (re-exports)

Make `import mypkg as m` expose the “nice” surface, while keeping internals private.

```python
# mypkg/__init__.py
from .core import run, Config     # what users should import
__all__ = ["run", "Config"]       # controls `from mypkg import *`

```

Benefits: discoverable API, stable imports (`from mypkg import run`) even if internals move.

## 2) To store package metadata

Expose `__version__` (and friends) in one place.

```python
# mypkg/__init__.py
from importlib.metadata import version, PackageNotFoundError
try:
    __version__ = version("mypkg")
except PackageNotFoundError:  # editable / source tree
    __version__ = "0.0.0.dev0"

```

## 3) Light initialization hooks

Very small, safe setup (e.g., default logging level, plugin registration).

```python
# mypkg/__init__.py
import logging
logger = logging.getLogger(__name__)
logger.addHandler(logging.NullHandler())  # good library hygiene

```

## 4) Lazy / compatibility shims

Delay heavy imports or keep old paths working.

```python
# PEP 562: attribute access on the package
def __getattr__(name):
    if name == "legacy_fn":
        from .compat import legacy_fn
        return legacy_fn
    raise AttributeError(name)

```

## 5) Explicit packages (avoid accidental namespaces)

If you **want a single distribution** and a normal package, keep an (even empty) `__init__.py` so your folder isn’t merged into a namespace with other distributions of the same top-level name.

---

# When NOT to use it

- **Namespace packages across multiple dists**
    
    If you intentionally split a top-level name (e.g., `acme.analytics`, `acme.tools`) into separate wheels that should “merge” at runtime, **omit** `__init__.py` at the **namespace level** (`acme/`), per **PEP 420**. Subpackages that aren’t shared can still have `__init__.py`.
    
    ```
    acme/            # no __init__.py  ← namespace package
      analytics/     # has __init__.py (regular subpackage)
      tools/         # has __init__.py
    
    ```
    

---

# Best practices

- Keep `__init__.py` **small and fast**. Avoid I/O, network calls, or heavy imports—users pay that cost on _every_ import.
- Use it to **re-export** the public API and hide internals; set `__all__` if you support star-imports.
- Provide `__version__` (read via `importlib.metadata` or a single `version.py`).
- Avoid complex logic that can cause **circular imports**; import locally inside functions if needed.
- For libraries, add a **NullHandler** to logging so `import mypkg` doesn’t configure global logging.
- Document your intended imports in README: e.g., “`from mypkg import run`”.

---

# Minimal layouts

**Typical single-package project**

```
mypkg/
  __init__.py        # re-exports, __version__, light setup
  core.py            # real implementation
  cli.py

```

**Expose API**

```python
# mypkg/__init__.py
from .core import run, Config
__all__ = ["run", "Config"]
from importlib.metadata import version
__version__ = version("mypkg")

```

**CLI entry point in pyproject**

```toml
[project.scripts]
mypkg = "mypkg.cli:main"

```

---

## TL;DR

- `__init__.py` turns a folder into a **regular package** and is the right place to define your **public API, version, and light init**.
- Omit it **only** when you purposely want a **namespace package** shared across multiple distributions.

---

Short answer: **sometimes, yes—but it depends why Python can’t “see” the package.**

## How Python finds your code

Python imports by searching **`sys.path`** (your venv, installed packages, current dir, etc.). A folder is importable as a package if either:

1. it has an **`__init__.py`** (a **regular package**), or
2. it’s an **implicit namespace package** (PEP 420) — i.e., a folder **without** `__init__.py`.

So, on modern Python (3.3+), **missing `__init__.py` does not automatically break imports**. But it can bite you in these common cases:

### When missing `__init__.py` _does_ cause trouble

- **Your folder isn’t on `sys.path`.**
    
    Example: you use the **`src/` layout** (`src/mypkg/...`). If you haven’t installed the project (e.g., `pip install -e .`) or added `src/` to `PYTHONPATH`, `import mypkg` fails—with or without `__init__.py`.
    
- **You expect package-level code to run.**
    
    Namespace packages (no `__init__.py`) can’t execute initialization code or expose `__version__`, re-exports, etc., at the top level. If your code relies on that, you need an `__init__.py`.
    
- **You want a _single_ distribution, not a merged namespace.**
    
    Without `__init__.py`, Python may **merge directories** with the same top-level name from multiple locations. If you don’t intend that, add `__init__.py` to make it a normal, self-contained package.
    
- **Old tools / patterns.**
    
    Some older tooling expects regular packages with `__init__.py`.
    

### When it’s fine to omit `__init__.py`

- You’re intentionally creating a **namespace package** split across multiple dists (e.g., `acme.analytics`, `acme.tools` published separately but sharing `acme/`).
- You don’t need package-level code or attributes.

### Other gotchas (not about `__init__.py` but often confused)

- **Running a module directly**: inside a package, relative imports break if you run a file like `python mypkg/module.py`. Prefer `python -m mypkg.module` (runs it as a module within the package).
- **Editable installs**: if imports hit the wrong copy, uninstall old installs and do `pip install -e .`.

## Practical guidance

- For most projects, **include `__init__.py`** in each package dir. It:
    - makes the package explicit and self-contained,
    - lets you define a clean public API (`from .core import run`),
    - lets you expose metadata (`__version__`), and
    - avoids accidental namespace merging.
- Ensure your code is **installed** (or `src/` is on `PYTHONPATH`) so Python can find it at all.

If you show me your folder layout (where your code lives) and how you’re running it, I’ll tell you exactly whether `__init__.py` is the fix or if it’s a `sys.path`/installation issue.

---

Short answer: **Yes, some files/folders appear automatically—**but not your source files like `__init__.py`. Python (and tools you run with Python) often **generate caches or outputs**.

## What Python auto-creates

- **`__pycache__/` + `.pyc`** — created on **import** to cache compiled bytecode (faster imports).
    - Safe to delete; they’ll be regenerated.
    - Disable if needed: set env var `PYTHONDONTWRITEBYTECODE=1` or run `python -B ...`.

## What common tools auto-create

- **Pytest:** `.pytest_cache/` (last-failed tests, timings). Delete or `pytest --cache-clear`.
- **Jupyter:** `.ipynb_checkpoints/` for notebook autosaves.
- **Pip/build backends:** `build/`, `dist/`, `.egg-info/` during packaging.
- **Matplotlib:** a font cache under your user cache dir (harmless).
- **Virtualenvs:** directory structures inside `.venv/` when you create a venv.

## What Python **does not** create for you

- **`__init__.py`** — never auto-created; you add it when you want a **regular package** (not a namespace).
- Your **source modules** (`.py` files) — you create these.

## Files your own code might create (by design)

If your code asks for them, Python will create them:

- `open("log.txt","w")` via logging handlers → creates/overwrites files.
- `sqlite3.connect("db.sqlite")` → creates DB if it doesn’t exist.
- Any explicit writes (pickle, CSV, images, etc.).

## Handy cleanup

```bash
# remove Python caches
find . -name "__pycache__" -type d -prune -exec rm -rf {} +
find . -name "*.py[co]" -delete
# remove pytest cache
rm -rf .pytest_cache
# remove build artefacts
rm -rf build dist *.egg-info

```

**TL;DR:** Auto-created stuff is mostly **caches and build artefacts**. You must still create real package files like `__init__.py` yourself.