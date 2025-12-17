#computer_theory #computer_science #python 

Here’s the practical diff, minus fluff:

# Big picture

- **`pyproject.toml`** (PEP 517/518/621): **Declarative, backend-agnostic** config file. Says _how to build_ your package and holds its **metadata** (name, version, deps). Tools: Setuptools, Hatch, Poetry, Flit, etc.
- **`setup.py`** (legacy Setuptools): **Imperative** Python script executed at build time to declare metadata and run custom logic.

# Why most projects use `pyproject.toml` now

- **Build isolation**: `pip` creates a clean env using `[build-system]` deps before building.
- **Standardized metadata** (PEP 621): no executable code needed for name/version/deps.
- **Backend choice**: swap Setuptools for Hatch/Poetry/Flit without changing the interface.
- **Editable installs** work with modern backends (PEP 660).

# When you might still need `setup.py`

- Rare, **custom build logic** that your backend can’t express via hooks/config.
    
    (Even then, modern backends usually have plugin/hook points—prefer those.)
    
- Old tooling expecting `python setup.py sdist bdist_wheel` (discouraged; use `pip build`/`python -m build`).
    

# Side note

- **`setup.cfg`**: declarative Setuptools config (no code). Many projects used `setup.cfg` as a bridge; today you can put the same metadata directly in `pyproject.toml`.

---

## Minimal examples

### Setuptools (modern, recommended)

```toml
# pyproject.toml
[build-system]
requires = ["setuptools>=61", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "mypkg"
version = "0.1.0"
description = "Example"
readme = "README.md"
requires-python = ">=3.10"
dependencies = ["requests>=2.31"]

```

Build/install:

```bash
pip install .
# or
python -m build

```

### Hatchling or Poetry (swap backend, same interface)

```toml
[build-system]
requires = ["hatchling>=1.24"]
build-backend = "hatchling.build"

[project]
name = "mypkg"
version = "0.1.0"
dependencies = ["requests>=2.31"]

```

### Legacy Setuptools (not recommended)

```python
# setup.py
from setuptools import setup
setup(
    name="mypkg",
    version="0.1.0",
    install_requires=["requests>=2.31"],
)

```

---

## TL;DR

- Use **`pyproject.toml`** for new work: it’s standard, reproducible, and backend-agnostic.
- Keep **`setup.py`** only if you truly need imperative build steps that your chosen backend can’t do (which is rare).