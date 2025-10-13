#computer_theory #computer_science #python 


Here are the PEPs I’d keep on your mental checklist—grouped by what they impact and why they matter in day-to-day projects.

# Packaging & distribution

- **PEP 517 – Build backend interface**: Standard hooks that tools like `pip` call to build wheels.
- **PEP 518 – `pyproject.toml` (build-system)**: Declare your build backend + build deps (build isolation).
- **PEP 621 – Project metadata in `pyproject.toml`**: Standard fields for name/version/dependencies, so you can drop `setup.py`.
- **PEP 660 – Editable installs for PEP 517**: `pip install -e .` works with modern backends (no `setup.py` needed).
- **PEP 440 – Versioning**: Canonical version scheme & specifiers (you just asked about this).
- **PEP 508 – Dependency specifiers**: The grammar for `requests>=2.31; python_version<'3.12'`.
- **PEP 503 – Simple repository API**: How `pip` talks to package indexes (mirrors, private indexes).
- **PEP 427 – Wheel format** & **PEP 425 – Wheel tags**: What a wheel is and how compatibility tags work.
- **PEP 541 – Package name conflicts on PyPI**: Rules for invalid/abandoned projects (useful if you ever need a name transfer).

# Imports & project layout

- **PEP 420 – Namespace packages**: Packages without `__init__.py` (handy for mono-repos/multi-dist layouts).
- **PEP 451 – ModuleSpec** (import system): Foundation for loaders/finders; matters when you write custom import hooks.
- **PEP 420 + packaging** together let you split a logical package across multiple distributions.

# Typing (very relevant for modern Python)

- **PEP 484 – Type hints**: The core typing system.
- **PEP 561 – Distributing typing info**: How libraries ship `.pyi` / inline hints so users get types.
- **PEP 585 – Built-in generics**: `list[int]` instead of `typing.List[int]`.
- **PEP 604 – Union types**: `int | str` instead of `typing.Union[int, str]`.
- **PEP 563 / PEP 649 – Deferred annotations**: Treat annotations as strings / lazy evaluation (affects runtime typing/ORMs). Modern Python effectively defers evaluation via `from __future__ import annotations` and newer semantics.
- **PEP 695 – Type parameter syntax**: Native `class Box[T]: ...` (Python 3.12+)—cleaner generics.
- **PEP 544 – Protocols**: Structural typing (`SupportsRead`, etc.)—great for testability and plugin architectures.

# Runtime & performance tools

- **PEP 703 – GIL-free CPython (opt-in build)**: Tracks the no-GIL effort (longer-term concurrency/perf implications).
- **PEP 669 – Low-impact monitoring**: Hook for profilers/coverage tools with less overhead.

# Virtual environments & execution

- **PEP 405 – `venv`**: Standard virtual environments in the stdlib.
- **PEP 582 (proposed)**: `__pypackages__` local deps (not accepted—good to know when you see debates/blog posts).

# Style & docs (team hygiene)

- **PEP 8 – Style guide**: Lints/formatters align to this.
- **PEP 257 – Docstring conventions**: How to write/structure docstrings (you asked earlier).

---

If you tell me your stack (e.g., **Hatch/Poetry vs setuptools**, Python version target, and whether you need **typed** packages), I’ll give you a minimal, production-ready `pyproject.toml` plus a “compliance checklist” mapping these PEPs to concrete settings and CI checks.