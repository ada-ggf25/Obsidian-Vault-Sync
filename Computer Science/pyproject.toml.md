#computer_science #virtual_environment

`pyproject.toml` (modern packaging metadata)

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
    

---

## Which should _you_ use?

- **Conda workflow?** Keep **`environment.yml`** as the source of truth. You can still list extra pip deps inside it.
    
- **Pure pip/venv app?** Use **`pyproject.toml`** for metadata + deps (modern), and generate a **`requirements.txt`** (from `pip freeze`) for deployment/CI reproducibility.
    
- **Publishing a library?** **`pyproject.toml`** is essential. Avoid committing a frozen `requirements.txt` for libraries (let installers resolve). Provide **extras** instead.
    
- **Need native/system libs (CUDA/GDAL/MKL)?** Prefer **Conda** → `environment.yml`.
    

### Quick patterns

- **App (pip/venv):**
    
    - Declare runtime deps in `pyproject.toml`.
        
    - Lock for deploy: `pip-compile` (pip-tools) → `requirements.txt`.
        
- **Library:**
    
    - `pyproject.toml` only (plus optional `requirements-dev.txt` for tests).
        
- **Conda-first project:**
    
    - `environment.yml` is primary; avoid duplicate lists in `requirements.txt` to prevent drift.
        

If you tell me your project type (library vs app) and tool preference (pip/venv, Conda, Poetry), I’ll sketch the exact file templates and commands for your setup.