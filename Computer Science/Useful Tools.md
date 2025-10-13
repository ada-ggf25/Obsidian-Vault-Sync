#python #computer_science #new #new_tools 

[[UV]]
[[Ruff]]
[[Python Checkers]]

Totally—here’s a tight, modern toolkit you’ll actually use (with **uv** + **Ruff** at the core).

# Core (ship these on every repo)

- **Ruff** — linter + formatter + import sorter + some pyupgrade.
    
    `uvx ruff check . && uvx ruff format .`
    
- **pytest** — testing; add **pytest-cov** for coverage.
    
    `uv add --dev pytest pytest-cov`
    
- **mypy** _or_ **Pyright** — static types (pick one; Pyright is faster in editors).
    
    `uv add --dev mypy` (Pyright runs via `uvx pyright`)
    
- **pre-commit** — runs Ruff/format/tests before commits.
    
    `uvx pre-commit install`
    
- **build** + **twine** — standards-compliant builds & upload.
    
    `uvx build ; uvx twine check dist/*`
    

# Dependency & packaging helpers

- **uv** (Astral) — fast resolver, envs, and tool runner (`uvx`). Use it instead of pip/venv.
    
    `uv init ; uv add requests`
    
- **pip-tools** — lock constraints for apps/libraries (if you prefer pip-style).
    
    `uv add --dev pip-tools ; uv run pip-compile`
    
- **hatch** _or_ **poetry** — if you want an all-in-one project manager (versioning, builds, scripts).
    

# Quality, security, hygiene

- **pip-audit** _(PyPA)_ or **safety** — scan vulns in deps.
    
    `uvx pip-audit`
    
- **bandit** — static security checks on your code.
    
    `uvx bandit -r .`
    
- **nbqa** — run Ruff/mypy/black on notebooks.
    
    `uvx nbqa ruff .`
    
- **commitizen** — conventional commits & automated version bumps/changelogs.
    
    `uvx cz init`
    

# Docs

- **MkDocs** + **mkdocstrings** (or **Sphinx**) — easy, pretty docs from docstrings.
    
    `uv add --dev mkdocs-material mkdocstrings[python]`
    

# Performance & profiling

- **py-spy** — low-overhead sampling profiler (production friendly).
    
    `uvx py-spy top -- python app.py`
    
- **Scalene** — CPU/GPU/alloc profiling with line-level detail.
    
    `uvx scalene script.py`
    

# CLI & DX niceties

- **Typer** (or Click) — great CLIs with type hints.
    
    `uv add typer[all]`
    
- **rich** / **textual** — pretty logs/CLIs/dashboards.
    
- **nox** (or tox) — matrix testing across Python versions.
    
    `uvx nox -s tests-3.11`
    

# Notebooks / data projects

- **Jupytext** — notebooks as plain .py/.md (version-control friendly).
    
    `uvx jupytext --sync nb.ipynb`
    
- **ipykernel** — per-project kernels: `uv add --dev ipykernel ; uv run python -m ipykernel install --user -–name myproj`
    
- **ruff-lsp** (Astral) — super fast LSP for editors (VS Code, Cursor, etc.).
    

---

## Drop-in configs (put in `pyproject.toml`)

```toml
[tool.ruff]
line-length = 100
target-version = "py311"
lint.select = ["E","F","I","B","UP","W","RUF","PL"]
lint.ignore = ["E501"]  # if you want Ruff-formatter to handle wrapping

[tool.mypy]
python_version = "3.11"
strict = true
ignore_missing_imports = true

[tool.pytest.ini_options]
addopts = "-q --color=yes --cov=yourpkg --cov-report=term-missing"
testpaths = ["tests"]

```

### `.pre-commit-config.yaml`

```yaml
repos:
  - repo: <https://github.com/astral-sh/ruff-pre-commit>
    rev: v0.6.8
    hooks:
      - id: ruff
      - id: ruff-format
  - repo: <https://github.com/pre-commit/mirrors-mypy>
    rev: v1.11.2
    hooks: [{ id: mypy }]
  - repo: <https://github.com/psf/black>
    rev: 24.8.0
    hooks: [{ id: black }]   # optional if you prefer Black over Ruff formatter

```

---

## Quick “happy path” with **uv**

```bash
uv init
uv add ruff pytest pytest-cov mypy typer rich
uvx pre-commit install
uvx ruff check . && uvx ruff format .
uv run pytest
uvx pip-audit

```

If you tell me the kind of project (library vs app, notebooks, CI target), I’ll spit out a tailored starter repo with ready-to-run configs and `pre-commit` hooks.