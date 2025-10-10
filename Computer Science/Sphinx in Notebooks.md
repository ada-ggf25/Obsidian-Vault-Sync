#computer_science #python 

Yep—Sphinx can build docs **from notebooks**. Two popular ways:

# Option A: MyST-NB (Markdown + notebooks, recommended)

**Install**

```bash
pip install sphinx myst-nb furo sphinx-autobuild

```

**[conf.py](http://conf.py)**

```python
extensions = [
    "myst_nb",                # notebook support
    "sphinx.ext.autodoc",
    "sphinx.ext.napoleon",
]
html_theme = "furo"

# Execution options (pick what you want)
nb_execution_mode = "auto"      # "off" | "force" | "cache" | "auto"
nb_execution_timeout = 120
nb_render_image_options = {"align": "center"}

```

**Add notebooks to your toctree ([index.md](http://index.md))**

````markdown
# Docs

```{toctree}
:maxdepth: 2
intro
notebooks/example.ipynb

````

````

**Build / live preview**
```bash
sphinx-build -b html . _build/html
sphinx-autobuild . _build/html

````

**Cell control (in notebook metadata or Markdown):**

- Hide code: set cell tag `hide-input`
- Don’t execute: tag `remove-cell` or set `nb_execution_mode: "off"`
- Per-cell skip: tag `raises-exception` (let exceptions pass as examples)

Tip: Pair with **Jupytext** to edit notebooks as `.md`/`.py` and keep outputs clean.

---

# Option B: nbsphinx (direct .ipynb rendering)

**Install**

```bash
pip install sphinx nbsphinx furo sphinx-autobuild

```

**[conf.py](http://conf.py)**

```python
extensions = ["nbsphinx"]
html_theme = "furo"
nbsphinx_execute = "auto"   # "never" | "always" | "auto"
nbsphinx_timeout = 120

```

**Toctree (index.rst or [index.md](http://index.md) via MyST)**

```
.. toctree::
   :maxdepth: 2

   notebooks/example.ipynb

```

---

## Common patterns & tips

- Keep heavy notebooks cached:
    - MyST-NB: `nb_execution_mode = "cache"` (creates `.jupyter_cache`)
    - nbsphinx: use `jupyter_cache` plugin or set `nbsphinx_execute="never"` for static outputs
- For **doctests in docs**: add `sphinx.ext.doctest` (works in text blocks, not code cells)
- For **API + notebooks**: combine `autodoc`/`autosummary` with MyST-NB pages
- CI builds: pin versions in `docs/requirements.txt` and run `sphinx-build -b html docs docs/_build/html`

---

### Minimal working structure

```
repo/
  docs/
    conf.py
    index.md
    notebooks/
      example.ipynb

```

That’s it—drop your `.ipynb` in the toctree and build. If you share your repo layout, I can spit out a ready-to-build `conf.py` and `index.md` tailored to it.