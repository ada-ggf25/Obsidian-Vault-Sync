#computer_science #virtual_environment

Here’s what each file is for, how they differ, and when you’d use them.

# 1) `environment.yml` (Conda)

- **Purpose:** Describe a **Conda environment**: Python version, Conda packages (and channels), and also pip packages if needed.
    
- **Who reads it:** `conda` (or `mamba`).
    
- **Typical use:** Data science / scientific stacks where you need non-Python libs (MKL, CUDA, GDAL) or precise Conda channels.
    
- **Create / use:**
    
    ```bash
    conda env create -f environment.yml
    conda env update -f environment.yml --prune
    ```
    
- **Example:**
    
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

### Quick patterns

- **App (pip/venv):**
    
    - Declare runtime deps in `pyproject.toml`.
        
    - Lock for deploy: `pip-compile` (pip-tools) → `requirements.txt`.
        
- **Library:**
    
    - `pyproject.toml` only (plus optional `requirements-dev.txt` for tests).
        
- **Conda-first project:**
    
    - `environment.yml` is primary; avoid duplicate lists in `requirements.txt` to prevent drift.
        

If you tell me your project type (library vs app) and tool preference (pip/venv, Conda, Poetry), I’ll sketch the exact file templates and commands for your setup.