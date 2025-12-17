#python #computer_theory #computer_science #virtual_environment

`pip install -e .` = **editable (a.k.a. “develop”) install**.

## What it does

- Installs your project **into the environment** _without copying the code_.
- Instead, it installs a small “link” (PEP 660 editable wheel / legacy `.egg-link`/`.pth`) that points to your **source tree**.
- Your project’s **dependencies are installed normally**.
- Because the interpreter imports your code **from your working folder**, any **file edits take effect immediately**—no reinstall needed.

## When to use it

- Day-to-day development: run `pytest`, notebooks, CLIs, or other projects that import your package while you keep editing the source.
- Mono-repo or multi-package setups where packages depend on each other locally.

## Typical workflow

```bash
# from your project root (where pyproject.toml lives)
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\\Scripts\\activate
pip install -e .            # install in editable mode
python -c "import yourpkg; print(yourpkg.__version__)"
# edit code... run tests... no reinstall needed

```

### With extras (e.g., dev tools)

```bash
pip install -e .[dev]       # installs extras listed under [project.optional-dependencies]

```

## How it works under the hood

- **Modern backends (PEP 660)** like Setuptools (≥61), Hatchling, Flit, Poetry build a tiny **editable wheel** that maps imports to your source directory.
- **Older Setuptools** used a lightweight `.egg-link` file. Behavior is similar from a user perspective.

## Common gotchas

- **Imports reflect the workspace**: if you move/rename packages, restart Python so import caches reset.
    
- **Package name vs import name** can differ (`my-project` on PyPI vs `import my_project`).
    
- If you also have a **non-editable copy** elsewhere on `PYTHONPATH`, you might import the wrong one—clean old installs with:
    
    ```bash
    pip uninstall yourpkg
    pip install -e .
    
    ```
    
- **Data files** generated at build time may not update automatically if your backend creates them only on non-editable builds.
    

## How to “turn it off”

Switch back to a regular install:

```bash
pip uninstall yourpkg
pip install .

```

## In Jupyter

Use the IPython-aware command so it installs into the **current kernel**:

```python
%pip install -e .

```

If you share your `pyproject.toml`, I can show the exact `extras`/dev-tool setup so `pip install -e .[dev]` gives you linting, tests, and type-checking out of the box.

---

No worries—here’s the simplest way to grok `pip install -e .` (“editable install”).

# What it does (in one line)

Installs your package **without copying the code**. Python imports **directly from your working folder**, so your edits take effect **immediately**—no reinstall needed.

# Compare quickly

|Action|Normal `pip install .`|Editable `pip install -e .`|
|---|---|---|
|Where code lives after install|Copied into site-packages|A tiny link/adapter points to your source tree|
|After you edit a `.py` file|Changes **don’t** affect the installed package until you reinstall|Changes are **instantly** visible on next import/run|
|Good for|End users, CI, releases|**Active development**|

# When to use it

- You’re developing a package and want to run its CLI/tests/notebooks while editing the code.
- Multi-repo/mono-repo where packages depend on each other locally.

# How to use (step-by-step demo)

```bash
# 0) from your project root (has pyproject.toml or setup.py)
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\\Scripts\\activate
pip install -e .                   # editable install
python -c "import yourpkg; print(yourpkg.__file__)"

```

You’ll see a path that points into **your source folder**, not site-packages.

Now edit `yourpkg/core.py` and run the import again—changes show up immediately.

# What gets installed?

- **Modern (PEP 660)**: your backend (setuptools/hatch/flit/poetry) builds a tiny **editable wheel** that maps imports to your source directory.
- **Older setuptools**: drops an `.egg-link` or `.pth` file into site-packages containing the path to your project folder.

# Typical layout (works great with `e`)

```
yourpkg/
  pyproject.toml    # or setup.py
  src/yourpkg/      # ← source (recommended “src/” layout)
  tests/

```

Editable install tells Python to import from `src/yourpkg` (or `yourpkg/` if not using src layout).

# Common gotchas (and fixes)

- **Didn’t pick up changes?** You may have an old non-editable copy shadowing it. Fix:
    
    ```bash
    pip uninstall yourpkg
    pip install -e .
    
    ```
    
- **Renamed/moved modules**: Python caches imports; restart the interpreter/kernel.
    
- **Data built at install time**: if your backend generates files only for non-editable builds, those won’t update automatically.
    
- **Not in project root**: run `pip install -e .` from the folder that has your `pyproject.toml`/`setup.py`.
    

# Uninstall / switch back

```bash
pip uninstall yourpkg
pip install .   # regular (non-editable) install

```

# Handy extras

- With dev tools:
    
    ```bash
    pip install -e .[dev]   # if you define a [project.optional-dependencies].dev
    
    ```
    
- In Jupyter, prefer:
    
    ```python
    %pip install -e .
    
    ```
    

**TL;DR:** `-e` creates an install that _points_ at your working tree. It’s for development so you can edit → run → edit → run without reinstalling every time.