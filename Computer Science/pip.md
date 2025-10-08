 #python #computer_science #virtual_environment

Here’s a crisp pip primer + a practical cheat sheet.

# What is `pip`?

`pip` is Python’s package manager. It installs Python libraries from the **Python Package Index (PyPI)** (and other indexes) into your Python environment.

# When to use `pip`

- ✅ Installing **Python** libraries for apps, scripts, notebooks.
    
- ✅ Managing deps in a **virtual environment** (recommended).
    
- ❌ Not for system tools like Git or Node (use OS package managers).
    
- ❌ Not for non-Python binaries or drivers.
    

Pro tip (Windows/WSL/macOS/Linux): prefer **`python -m pip`** (or `py -m pip` on Windows) to ensure you’re using pip for the exact Python you intend.

---

# Quick start (safe pattern)

```bash
# Create and use a virtual environment
python -m venv .venv
# Windows PowerShell:
.\.venv\Scripts\Activate.ps1
# macOS/Linux/WSL:
source .venv/bin/activate

# Upgrade pip itself
python -m pip install -U pip

# Install packages
python -m pip install requests numpy
```

---

# pip Cheat Sheet

## Basics

```bash
python -m pip --version                # show pip + python path
python -m pip install PACKAGE          # install latest
python -m pip install PACKAGE==1.2.3   # exact version
python -m pip install "PACKAGE>=1,<2"  # version range
python -m pip uninstall PACKAGE        # remove
python -m pip list                     # installed packages
python -m pip show PACKAGE             # details (location, version, deps)
python -m pip check                    # report broken requirements
```

## Requirements files

```bash
# freeze current env (exact versions) — great for apps
python -m pip freeze > requirements.txt

# install from requirements
python -m pip install -r requirements.txt

# use a constraints file (pins upper bounds without forcing installs)
python -m pip install -r requirements.in -c constraints.txt
```

## Editable / local / wheels

```bash
# editable/local dev install (updates reflect local edits)
python -m pip install -e .

# install from a local folder or wheel
python -m pip install ./path/to/pkg
python -m pip install dist/my_pkg-0.1.0-py3-none-any.whl
```

## From Git (VCS installs)

```bash
# branch
python -m pip install "git+https://github.com/OWNER/REPO.git@branch"

# tag/commit
python -m pip install "git+https://github.com/OWNER/REPO.git@v1.2.0"
python -m pip install "git+https://github.com/OWNER/REPO.git@<commit_sha>"

# subdir (pyproject in a subfolder)
python -m pip install "git+https://github.com/OWNER/REPO.git@branch#subdirectory=path/to/subpkg"
```

## Indexes, proxies, certs

```bash
# use a mirror / private index
python -m pip install PACKAGE -i https://pypi.org/simple
python -m pip install PACKAGE --extra-index-url https://my.index/simple

# behind a proxy
python -m pip install PACKAGE --proxy http://user:pass@host:port
```

## Caching & performance

```bash
python -m pip cache dir                # where cache lives
python -m pip cache info
python -m pip cache purge              # clear cache
python -m pip install PACKAGE --no-cache-dir   # bypass cache
```

## Build backends / PEP 517

```bash
# build sdist+wheel for your package (requires build)
python -m pip install build
python -m build
# install in isolated build env
python -m pip install .
```

## Constraints for reproducibility

```bash
# pin transitive deps without hard-coding them into requirements.in
echo "numpy<2" > constraints.txt
python -m pip install -r requirements.in -c constraints.txt
```

## Troubleshooting

```bash
# verbose output (see index URLs, resolution, etc.)
python -m pip install PACKAGE -v

# incompatible binary (e.g., on WSL/mac M1)?
python -m pip install --only-binary=:all: PACKAGE   # prefer wheels only
python -m pip install --no-binary=:all: PACKAGE     # force source build (needs compilers)

# conflicting deps? try a clean venv
deactivate  # if active
python -m venv .venv && source .venv/bin/activate
python -m pip install -U pip
python -m pip install -r requirements.txt
```

## Good practices (quick)

- Use **virtual environments** (`venv`, Conda, Poetry) per project.
    
- Commit **`requirements.txt`** (or `pyproject.toml`/`poetry.lock`) for apps.
    
- Prefer **wheels** (prebuilt binaries) when possible.
    
- Avoid `sudo pip` or system-wide installs (breaks OS Python).
    
- On Windows PowerShell, use:
    
    ```powershell
    py -m venv .venv
    .\.venv\Scripts\Activate.ps1
    py -m pip install -U pip
    py -m pip install PACKAGE
    ```
    

---

## `pip` vs. `conda` vs. `pipx` (when to use which)

- **pip**: Python libs from PyPI, inside a venv (most projects).
    
- **conda**: cross-language packages and native libs (e.g., CUDA, GDAL). You can still use `pip` inside a conda env.
    
- **pipx**: install **CLI apps** in isolated envs (e.g., `pipx install black`).
    

---

If you tell me your OS and project type (app, ML notebook, package), I’ll give you a minimal `requirements.txt` template and the exact commands to bootstrap your environment.