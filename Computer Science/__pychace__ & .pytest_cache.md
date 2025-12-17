#python #computer_theory #computer_science 

[[Commit or not Commit]]

Short answer: both are harmless **caches** you can delete anytime.

# `__pycache__/`

- Created by **Python itself** when a module is imported.
    
- Stores compiled bytecode files (`.pyc`), e.g. `mymod.cpython-312.pyc`.
    
- Purpose: **speed up imports** (skip recompiling `.py` each run).
    
- Safe to delete: **Yes**. Python will recreate them on next import.
    
- Keep them out of Git:
    
    ```
    __pycache__/
    *.py[cod]
    
    ```
    
- Turn off writing `.pyc` (not usually recommended):
    
    - Env var: `PYTHONDONTWRITEBYTECODE=1`
    - Or run: `python -B script.py`

# `.pytest_cache/`

- Created by **pytest** to speed/aid test runs.
    
- Keeps things like:
    
    - last-failed test list (for `k`, `-lf`, `-ff`)
    - node IDs, durations, selection state, plugin data
- Safe to delete: **Yes**. Pytest rebuilds it next run.
    
- Clean it via pytest: `pytest --cache-clear`
    
- Ignore in Git:
    
    ```
    .pytest_cache/
    
    ```
    

# Handy cleanup commands

```bash
# remove Python bytecode caches
find . -name "__pycache__" -type d -prune -exec rm -rf {} +
find . -name "*.py[co]" -delete

# clear pytest cache
pytest --cache-clear

```

**TL;DR:** they’re just performance/state caches for Python and pytest. Don’t commit them; delete freely if they get in the way.