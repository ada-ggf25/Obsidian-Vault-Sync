#computer_science #python 

Here’s a tight **Sphinx cheat sheet** you can stick next to your editor.

# Install & Quickstart

```bash
pip install sphinx furo myst-parser sphinx-autobuild
sphinx-quickstart          # answer prompts to create docs/

```

# Build / Serve

```bash
make html                  # build to _build/html
sphinx-build -b html . _build/html
sphinx-autobuild . _build/html   # live-reload while editing

```

# Core Files (created by quickstart)

```
docs/
  conf.py        # Sphinx config (Python)
  index.rst      # entry page (can use .md with MyST)
  _build/        # outputs
  _static/, _templates/  # assets & templates

```

# Minimal `conf.py` (good defaults)

```python
import os, sys
sys.path.insert(0, os.path.abspath("../src"))  # <— point to your package

extensions = [
    "myst_parser",             # Markdown (MyST)
    "sphinx.ext.autodoc",      # pull docstrings
    "sphinx.ext.napoleon",     # Google/NumPy style docstrings
    "sphinx.ext.autosummary",  # API summary tables
    "sphinx.ext.intersphinx",  # x-ref to external docs
    "sphinx.ext.viewcode",     # [source] links
]

autosummary_generate = True
html_theme = "furo"
intersphinx_mapping = {
    "python": ("<https://docs.python.org/3>", {}),
    "numpy": ("<https://numpy.org/doc/stable/>", {}),
}

```

# Index page (RST)

```
Welcome to MyProject
====================

.. toctree::
   :maxdepth: 2

   usage
   api

```

# Or use Markdown (MyST)

````markdown
# Welcome to MyProject

```{toctree}
:maxdepth: 2
usage
api

````

````

# API Docs (Autodoc + Autosummary)
**RST version**
```rst
API Reference
=============

.. autosummary::
   :toctree: _autosummary
   :recursive:

   mypkg
   mypkg.module_a
   mypkg.module_b

````

**Markdown (MyST) version**

````markdown
# API Reference

```{autosummary}
:toctree: _autosummary
:recursive:
mypkg
mypkg.module_a
mypkg.module_b

````

````

# Docstring Style (Napoleon)
```python
def add(a: int, b: int) -> int:
    """Add two numbers.

    Args:
        a: First number.
        b: Second number.

    Returns:
        Sum of a and b.
    """
    return a + b

````

# Common Directives & Roles

- **Headings (MyST)**: `#`, `##`, `###` …
    
- **Code blocks**:
    
    RST:
    
    ```
    .. code-block:: python
    
       print("hi")
    
    ```
    
    MyST:
    
    ````
    ```python
    print("hi")
    ````
    
- **Notes/Warnings**:
    
    RST: `.. note::`, `.. warning::`
    
    MyST:
    
    ```markdown
    :::note
    Be mindful...
    :::
    
    ```
    
- **Cross-refs**: `:mod:`mypkg.module``,` :class:`mypkg.Foo``
    
- **Images**: `.. image:: _static/img.png` (or MyST’s `![alt](_static/img.png)`)
    

# Live Editing Loop

```bash
sphinx-autobuild docs docs/_build/html
# browse <http://127.0.0.1:8000>

```

# Notebooks as Docs

```bash
pip install myst-nb

```

`conf.py`:

```python
extensions += ["myst_nb"]

```

Then add `.ipynb` files to your toctree; they’ll be executed during build (configurable).

# Doctest in Docs

```python
extensions += ["sphinx.ext.doctest"]

```

RST/MD blocks with `>>>` examples are executed; configure in `conf.py`:

```python
doctest_default_flags =  (1 << 0)  # or use doctest.ELLIPSIS etc. via Python

```

# PDF / EPUB

```bash
make latexpdf    # needs TeX distribution (TeX Live/MacTeX)
make epub

```

# Theming & UX

```bash
pip install furo  # or sphinx-rtd-theme, pydata-sphinx-theme
# conf.py
html_theme = "furo"
html_static_path = ["_static"]          # put custom CSS/JS here
# Optional: add copy buttons on code blocks
pip install sphinx-copybutton
extensions += ["sphinx_copybutton"]

```

# Typical Project Layout

```
repo/
  src/mypkg/...
  docs/
    conf.py
    index.md
    usage.md
    api.md
    _static/

```

# Read the Docs (free hosting)

1. Push `docs/` to GitHub/GitLab.
2. Add `docs/requirements.txt` with Sphinx deps.
3. Import the repo on [](https://readthedocs.org/)[https://readthedocs.org](https://readthedocs.org) → choose `docs/` as root (if needed).
4. RTD builds on each commit and hosts at `yourproject.readthedocs.io`.

# Handy Extensions (pick as needed)

- `sphinx.ext.autodoc.preserve_defaults` / `sphinx_autodoc_typehints` – tidy type hints
- `sphinx.ext.napoleon` – Google/NumPy docstrings
- `sphinx.ext.graphviz` / `sphinxcontrib.mermaid` – diagrams
- `sphinx.ext.todo` – TODOs in docs
- `sphinx.ext.duration` – build timing
- `sphinx.ext.linkcheck` – validate external links (`make linkcheck`)

# Troubleshooting Fast

- **Import errors**: ensure `sys.path` points to your package in `conf.py`.
- **No API pages**: add `autosummary_generate = True` and an `autosummary` block.
- **Theme not found**: `pip install` the theme and set `html_theme`.
- **CI builds differ**: pin versions in `docs/requirements.txt`.

---

Want me to produce a ready-to-build starter `docs/` (with `conf.py`, `index.md`, `api.md`, and autosummary wired to your package path)?