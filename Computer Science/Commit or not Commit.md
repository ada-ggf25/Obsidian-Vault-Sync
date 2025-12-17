#computer_science #github #git #python 

Short answer:

- **`__pycache__/` → DO NOT commit.**
    
    Just bytecode caches (`*.pyc`) created by Python; they’re regenerated automatically.
    
- **`.pytest_cache/` → DO NOT commit.**
    
    Pytest’s run cache (last-failed, durations, etc.); ephemeral and local.
    
- **`__init__.py` → YES, commit (usually).**
    
    It defines a **regular package** and is the right place to expose your package API, version, etc.
    
    **Only omit it** if you _intentionally_ use **namespace packages** (PEP 420) where multiple distributions share the same top-level package.
    

### Suggested `.gitignore`

```
# Python bytecode
__pycache__/
*.py[cod]

# pytest
.pytest_cache/

# virtual envs (common)
.venv/
venv/

```

### Rule of thumb

- **Commit code and package markers** (`__init__.py`).
- **Ignore caches and build artefacts** (`__pycache__`, `.pytest_cache`, `build/`, `dist/`, etc.).