#computer_science #virtual_environment #python 

# 2) `requirements.txt` (pip)

- **Purpose:** List **pip** packages to install into the current environment (often exact, frozen versions).
    
- **Who reads it:** `pip`.
    
- **Typical use:** Applications and simple projects; CI deploys; quick reproduction of a working env.
    
- **Create / use:**
    
    ```bash
    python -m pip freeze > requirements.txt   # pin exact versions
    python -m pip install -r requirements.txt
    ```
    
- **Notes:**
    
    - Order doesn’t imply resolution; pip resolves at install time.
        
    - For pinning _upper bounds_ without forcing installs, pair with a **constraints** file (`-c constraints.txt`).
        

# 3) `pyproject.toml` (modern packaging metadata)

- **Purpose:** The **standard project config** file for Python packages (PEP 518/621). Declares:
    
    - build system (`setuptools`, `hatchling`, `poetry`, `pdm`, etc.)
        
    - package metadata (name, version, authors)
        
    - **install requirements** (`dependencies`) and **optional/extras**
        
    - tool configs (ruff/black/mypy/etc.)
        
- **Who reads it:** Build backends and tooling (`pip` during build, Poetry/PDM/Hatch/Setuptools).
    
- **Typical use:** Libraries you publish (and increasingly, apps) with a clean, single source of truth.
    
- **Install / build:**
    
    ```bash
    # user of your package
    python -m pip install .
    
    # build distributions (requires 'build')
    python -m pip install build
    python -m build
    ```
    
- **Example (setuptools backend):**
    
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
      "uvicorn[standard]>=0.30"
    ]
    
    [project.optional-dependencies]
    dev = ["ruff", "pytest"]
    
    [tool.setuptools.packages.find]
    where = ["src"]
    ```
### Quick patterns

- **App (pip/venv):**
    
    - Declare runtime deps in `pyproject.toml`.
        
    - Lock for deploy: `pip-compile` (pip-tools) → `requirements.txt`.
        
- **Library:**
    
    - `pyproject.toml` only (plus optional `requirements-dev.txt` for tests).
        
- **Conda-first project:**
    
    - `environment.yml` is primary; avoid duplicate lists in `requirements.txt` to prevent drift.
        

If you tell me your project type (library vs app) and tool preference (pip/venv, Conda, Poetry), I’ll sketch the exact file templates and commands for your setup.