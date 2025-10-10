#computer_science #python 

[[Sphinx - Cheat Cheet]]
[[Sphinx in Notebooks]]

**Sphinx** is a documentation generator for Python (and more). You write in **reStructuredText** or **Markdown (via MyST)**, and Sphinx builds clean **HTML**, **PDF**, **EPUB**, and API docs by **auto-extracting docstrings**.

# Why use it?

- **Autodoc**: turns your Python docstrings into API pages
- **Cross-refs & search** out of the box
- **Themes & extensions** (Read the Docs theme, NumPy/Google docstring support, notebooks, diagrams, typehints)
- **Great for CI/Read the Docs** hosting

# 60-second start

```bash
pip install sphinx sphinx-autobuild furo myst-parser
sphinx-quickstart  # answer prompts (project name, author, etc.)

```

Edit `conf.py` to enable common extensions:

```python
extensions = [
    "myst_parser",             # Markdown (MyST)
    "sphinx.ext.autodoc",      # pull in docstrings
    "sphinx.ext.napoleon",     # Google/NumPy docstrings
    "sphinx.ext.autosummary",  # summary tables
    "sphinx.ext.intersphinx",  # cross-link to other projects
    "sphinx.ext.viewcode",     # source links
    "sphinx.ext.napoleon",
]
autosummary_generate = True
html_theme = "furo"            # or "sphinx_rtd_theme"

```

Point Sphinx at your code (so autodoc can import it):

```python
# in conf.py
import os, sys
sys.path.insert(0, os.path.abspath("../src"))  # adjust to your package path

```

Create pages in `index.rst` (or `index.md`):

```
Welcome to MyProject
====================

.. toctree::
   :maxdepth: 2

   usage
   api

```

Minimal API page (RST):

```
API reference
=============

.. autosummary::
   :toctree: _autosummary
   :recursive:

   mypkg.module
   mypkg.subpkg

```

Build docs:

```bash
make html           # or: sphinx-build -b html . _build/html
sphinx-autobuild . _build/html  # live-reload while editing

```

# With Markdown (MyST)

Use `.md` files and MyST directives:

````markdown
# API

```{autosummary}
:toctree: _autosummary
:recursive:
mypkg.module

````

````

# Typical docstring style (works with Napoleon)
```python
def add(a: int, b: int) -> int:
    """Add two numbers.

    Args:
        a: First.
        b: Second.

    Returns:
        Sum of a and b.
    """
    return a + b

````

# Useful extensions (pick as needed)

- `sphinx.ext.napoleon` — Google/NumPy docstrings
- `sphinx.ext.autodoc.typehints` / `sphinx_autodoc_typehints` — render type hints nicely
- `myst_nb` — execute notebooks as docs
- `sphinx.ext.doctest` — run doctests in docs
- `sphinx.ext.intersphinx` — link to Python/NumPy/Pandas docs
- `sphinx.ext.graphviz` / `sphinxcontrib.mermaid` — diagrams
- `sphinx-copybutton` — copy buttons on code blocks

# Read the Docs (free hosting)

- Add a `requirements.txt` (or `environment.yml`) with Sphinx deps
- Push repo with `docs/` to GitHub/GitLab
- Import project on [readthedocs.org](http://readthedocs.org) → it builds on each commit

# Common tips

- Keep code in `src/yourpkg` and set `sys.path` in `conf.py`.
- Use `autosummary` to auto-generate API stubs; commit the generated `_autosummary/` or re-generate in CI.
- For large projects, split pages: **Getting Started**, **User Guide**, **API**, **How-Tos**, **FAQ**, **Changelog**.
- Combine with `pytest --doctest-modules` to verify examples in docstrings.

If you want, tell me your repo layout and I’ll give you a ready-to-copy `conf.py`, `index.md`, and API stubs tailored to it.