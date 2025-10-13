#python #computer_science 

# Publish (the safe, modern way)

## 0) One-time setup

1. Add a minimal **`pyproject.toml`** (PEP 517/518/621):

```toml
[build-system]
requires = ["setuptools>=61", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "your-package-name"
version = "0.1.0"         # PEP 440
description = "..."
readme = "README.md"
requires-python = ">=3.10"
dependencies = []

```

1. Create accounts:

- **PyPI** and **TestPyPI** (test index).
- Make an **API token** (PyPI → Account settings → API tokens). Save it in your password manager.

## 1) Build

```bash
python -m pip install --upgrade build twine
python -m build              # creates dist/*.tar.gz and dist/*.whl
twine check dist/*           # verifies metadata/README rendering

```

## 2) Dry-run on TestPyPI

```bash
# set env var once in your shell
export TESTPYPI_TOKEN=pypi-AgENdGV...
twine upload --repository testpypi -u __token__ -p "$TESTPYPI_TOKEN" dist/*
# install to verify
python -m pip install -i <https://test.pypi.org/simple> your-package-name==0.1.0

```

## 3) Publish to PyPI

```bash
export PYPI_TOKEN=pypi-AgENdGV...
twine upload -u __token__ -p "$PYPI_TOKEN" dist/*

```

> Tip: bump the version each release (0.1.1, 0.2.0, etc.). Never overwrite an existing version.

## 4) (Optional) CI without tokens

Use **Trusted Publishers** from GitHub Actions to publish via OIDC (no long-lived token). Search “PyPI Trusted Publishers GitHub Actions” for the template workflow.

---

# “Unpublish” / remove / fix mistakes

There isn’t a true “unpublish” button after a grace period. You have these options:

## A) **Delete within ~24 hours**

- PyPI allows removing files/releases you just uploaded (UI or API) for about **24h**. Use the project’s “Manage” page to delete the erroneous files/release.

## B) **Yank a release** (preferred after 24h)

- Marks a version as problematic; `pip` will **avoid it** unless the user pins exactly to it.
    
- In the PyPI UI: go to the release → “Yank this release”, optionally add a reason.
    
- Or CLI (Twine ≥ 4):
    
    ```bash
    twine yank your-package-name 0.1.0 -r pypi -m "bad wheel"
    # undo:
    twine yank --unyank your-package-name 0.1.0 -r pypi
    
    ```
    

## C) **Supersede with a new version**

- Bump version and release a fixed build (most common real-world fix).

## D) **Full deletion of a project**

- Only possible if **no releases remain**. After the 24h window, getting all releases removed requires a **support request** and is granted only for exceptional reasons (legal/security/security keys leaked in sdist, etc.).

---

# Common gotchas & tips

- **Name lock-in**: Once a project name is used on PyPI, it’s essentially reserved. Choose carefully.
- **Reproducibility**: Pin build deps in `[build-system]` and consider a `constraints.txt` for CI.
- **Long descriptions**: Keep `README` in Markdown/reST and run `twine check` to catch rendering issues.
- **Wheels**: Ship wheels for your platforms if you have extensions; otherwise users may need compilers.
- **Test first** on **TestPyPI** to avoid irreversible mistakes.

Want me to turn your repo into a ready-to-publish package (with `pyproject.toml`, classifiers, `LICENSE`, `__init__.__version__`, and a GitHub Actions publish workflow)? Paste your folder structure and I’ll generate the files.