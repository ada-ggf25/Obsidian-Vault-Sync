#computer_science #virtual_environment #python 

Great question. Think of these three as different “sources of truth” for your project’s dependencies and packaging—each serves a distinct job:

# The 10-second versions

|File|What it is|Who uses it|When to use it|
|---|---|---|---|
|`environment.yml`|**Conda environment recipe** (Python + non-Python libs + channels; can include pip deps)|`conda` / `mamba`|Data/ML/scientific stacks or any project needing system/native packages (e.g., CUDA, GDAL, MKL).|
|`requirements.txt`|**Exact pip packages** to install (often frozen versions)|`pip`|Reproducible app/runtime installs with pip (CI, servers). Not ideal as the primary spec for a _library_.|
|`pyproject.toml`|**Modern project config** + packaging metadata (build backend, name/version, deps, extras)|`pip` (during build), `setuptools/poetry/hatch/pdm`|When you’re building a **Python package** (library or app) to install/distribute properly.|

---

# What each one looks like (tiny examples)

### 1) `environment.yml` (Conda)

```yaml
name: myproj
channels: [conda-forge]
dependencies:
  - python=3.11
  - numpy
  - pandas
  - pip
  - pip:
      - fastapi==0.115.*
      - uvicorn[standard]
```

Create/update:

```bash
conda env create -f environment.yml
conda env update -f environment.yml --prune
```

### 2) `requirements.txt` (pip)

```
fastapi==0.115.5
uvicorn[standard]==0.30.6
pandas==2.2.3
numpy==2.1.2
```

Install:

```bash
python -m pip install -r requirements.txt
# generate (freeze current env):
python -m pip freeze > requirements.txt
```

### 3) `pyproject.toml` (packaging & deps)

```toml
[build-system]
requires = ["setuptools>=68", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "myproj"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
  "fastapi>=0.115,<0.116",
  "uvicorn[standard]>=0.30",
]

[project.optional-dependencies]
dev = ["pytest", "ruff"]
```

Use/install/build:

```bash
python -m pip install .      # installs according to pyproject
python -m pip install build && python -m build   # makes wheel/sdist
```

---

# When should _you_ use which?

## If you’re using **Conda** (common for DS/ML and WSL):

- Make **`environment.yml` your source of truth**.
    
- Put any pip-only packages under the `pip:` section.
    
- Only export `requirements.txt` if you must deploy to a **pip-only** environment:
    
    ```bash
    conda list --export   # conda-style (not for pip)
    # or better: pip freeze in the activated conda env
    python -m pip freeze > requirements.txt
    ```
    

## If you’re building a **library** to publish:

- Use **`pyproject.toml`** to declare metadata + dependencies.
    
- Don’t pin exact versions in a library; use ranges.
    
- Avoid `requirements.txt` for runtime deps; instead provide
    
    - an optional `requirements-dev.txt` for tests/dev, or
        
    - `[project.optional-dependencies]` (extras) in `pyproject.toml`.
        

## If you’re shipping an **application** with **pip/venv**:

- Put runtime deps in **`pyproject.toml`** (modern approach).
    
- Generate a **locked** `requirements.txt` for deployment/CI:
    
    - with `pip-tools` (`pip-compile`) or `pip freeze`.
        
- CI/servers then do: `pip install -r requirements.txt`.
    

---

# Common pitfalls (and how to avoid them)

- **Duplicating the same list in multiple files → drift.**  
    Pick a primary spec:
    
    - Conda-first → `environment.yml` primary; generate others when needed.
        
    - Pip/venv app → `pyproject.toml` primary; generate `requirements.txt` for deploy.
        
- **Using `requirements.txt` for a library.**  
    Prefer `pyproject.toml` with dependency ranges.
    
- **Mixing conda & pip haphazardly.**  
    If using Conda, put pip packages in the `pip:` block of `environment.yml` to keep the recipe coherent.
    

---

# Quick decision cheat

- Need CUDA/GDAL/MKL or complex native deps? → **`environment.yml`**
    
- Publishing/installable package (on PyPI or internal index)? → **`pyproject.toml`**
    
- Deploying an app to pip-only environments/CI and want exact reproducibility? → **`requirements.txt`** (generated from your primary spec)
    

If you tell me your project type (library vs app) and tool preference (Conda vs pip/venv), I’ll give you a minimal, ready-to-use set of files tailored to your stack.

## Environment files in 2025 (Python 3.13): `pyproject.toml`, `environment.yml`, and (maybe) `requirements.txt`

  

### Source of truth

- **Use `pyproject.toml` as the single source of truth** for your project’s runtime dependencies and optional extras.

- Avoid duplicating dependency lists across files.

  

### When to use `environment.yml`

Use **`environment.yml`** to create a **conda** environment that pins Python and a few heavy scientific libs. Then install your package (and its deps) from `pyproject.toml` **inside** that env.

  

**Recommended `environment.yml`:**

```bash

    name: mpm-lecture

    channels:

      - conda-forge

    dependencies:

      - python=3.13

      - pip

      - numpy>=2.1

      - pandas>=2.2.3

      - matplotlib>=3.10

      # - scipy>=1.14.1   # include only if actually used

      - pip:

          - -e .[dev]    # installs your package + extras declared in pyproject.toml

```

  

**Update the env:**

```bash

    conda env remove -n mpm-lecture -y

    conda env create -f environment.yml

    conda activate mpm-lecture

```

> `conda env update -f environment.yml` can work but is less predictable for students.

  

### Do we still need `requirements.txt`?

  

- Often **no**. For pip/venv users:

    `pip install -e .[dev]`

This reads dependencies directly from `pyproject.toml`.

  

- If you need a pinned list (CI/workshops), **generate** it instead of hand-maintaining:

    `pip-compile -o requirements.txt pyproject.toml`

Or keep pins in a **constraints** file and install with:

    `pip install -c constraints.txt -e .[dev]`

  

### Optional/extra packages (prefer extras over multiple req files)

  

In `pyproject.toml`:

```bash

    [project.optional-dependencies]

    dev = ["pytest>=8", "ruff>=0.5"]

    viz = ["scikit-image>=0.24"]

```

Usage:

- pip/venv: `pip install -e .[dev,viz]`

- conda (`environment.yml`): put `-e .[dev,viz]` under the `pip:` block.

  

### Why this setup?

- **Single source of truth** (`pyproject.toml`) → fewer sync errors.

- **Conda** handles Python/system libs; **pip** installs your package + declared deps.

- **Extras** make optional components explicit and easy to install.