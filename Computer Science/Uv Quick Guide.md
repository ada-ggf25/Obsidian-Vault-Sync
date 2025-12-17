#computer_science #virtual_environment 

Do you mean **uv** by **Astral** (the team behind Ruff)? It’s a super-fast Python package & project manager that can replace `pip`, `pip-tools`, `pipx`, and most Poetry/virtualenv workflows. Here’s a crisp, no-nonsense quickstart.

# Install

- **macOS/Linux**
    
    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```
    
- **Windows (PowerShell)**
    
    ```powershell
    powershell -ExecutionPolicy Bypass -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```
    

(You can also `winget install astral-sh.uv`.) ([Astral Docs](https://docs.astral.sh/uv/getting-started/installation/?utm_source=chatgpt.com "Installation | uv - Astral Docs"))

# Core workflow (new project)

```bash
# 1) Create a project (writes pyproject.toml)
uv init myproj && cd myproj

# 2) Add dependencies (creates/updates uv.lock)
uv add numpy pandas

# 3) Sync env (creates .venv and installs exact locked deps)
uv sync

# 4) Run code/commands with the project env
uv run python main.py
uv run pytest
```

([Astral Docs](https://docs.astral.sh/uv/guides/projects/?utm_source=chatgpt.com "Working on projects | uv - Astral Docs"))

# Managing Python itself

uv can **install & pin Python versions**—no pyenv needed:

```bash
# Install latest (or specific) Python
uv python install
uv python install 3.12

# Pin a project to a version
uv python pin 3.12

# List / select versions
uv python list
```

If a pinned version is missing, uv will auto-install it. ([Astral Docs](https://docs.astral.sh/uv/guides/install-python/?utm_source=chatgpt.com "Installing and managing Python | uv - Astral Docs"))

# One-off tools (like pipx) with `uvx`

```bash
# Run a tool without installing globally
uvx ruff --version
uvx black --check .
# Or install a persistent tool
uv tool install ruff
```

`uvx` == `uv tool run`. Tools run in an isolated temp env. ([Astral Docs](https://docs.astral.sh/uv/concepts/tools/?utm_source=chatgpt.com "Tools | uv - Astral Docs"))

# Useful commands at a glance

```bash
uv add <pkg>            # add dependency
uv remove <pkg>         # remove dependency
uv sync                 # resolve + install per uv.lock
uv lock                 # (re)solve lockfile manually (usually not needed)
uv run <cmd>            # run within the project env
uv build                # build sdist/wheel
uv export -o req.txt    # export pinned requirements.txt (for CI/compat)
uv self update          # update uv
```

([Astral Docs](https://docs.astral.sh/uv/?utm_source=chatgpt.com "uv - Astral Docs"))

# Migrating an existing project

```bash
# From requirements.txt
uv init                 # if you don’t have a pyproject yet
uv add -r requirements.txt
uv sync
```

Keeps a lockfile (`uv.lock`) for reproducible builds. ([Reddit](https://www.reddit.com/r/learnpython/comments/1md4hyj/i_think_i_have_to_admit_im_confused_by_how_to/?utm_source=chatgpt.com "I think I have to admit I'm confused by how to correctly use uv"))

# Why use uv?

- **Speed** (10–100× faster dependency resolution/install).
    
- **Unified tool**: envs, deps, Python versions, builds, and tools in one CLI. ([Astral Docs](https://docs.astral.sh/uv/?utm_source=chatgpt.com "uv - Astral Docs"))
    

If you tell me your OS/shell and whether you want to migrate a Poetry/pip/conda project, I’ll tailor the exact commands.