#python #computer_science #computer_theory #virtual_environment

Here’s a compact, high-signal **`pip` cheat sheet** (install + most-used commands). Use `python -m pip ...` to be sure you hit the right Python.

# Basics

```bash
pip install PACKAGE              # latest
pip install PACKAGE==1.2.3       # exact
pip install "PACKAGE>=1.2,<2"    # range (PEP 440)
pip install -U PACKAGE           # upgrade (same as --upgrade)
pip uninstall PACKAGE            # remove
pip list                         # installed packages
pip show PACKAGE                 # details (version, location, deps)
pip freeze > requirements.txt    # exact, reproducible set
pip check                        # broken/missing deps

```

# Editable / local / files

```bash
pip install -e .                 # editable (dev mode, from current folder)
pip install .                    # build & install from local project
pip install path/to/pkg.whl      # wheel file
pip install path/to/pkg.tar.gz   # sdist (source archive)
pip install -r requirements.txt  # from a requirements file
pip install -c constraints.txt   # constrain versions during install

```

# Extras, pre-releases, no deps

```bash
pip install "package[extra1,extra2]"
pip install --pre PACKAGE        # allow pre-releases
pip install --no-deps PACKAGE    # don’t install dependencies

```

# Indices, links, and trusted hosts

```bash
pip install --index-url <https://pypi.org/simple> PACKAGE
pip install --extra-index-url <https://my.index/simple> PACKAGE
pip install --find-links ./wheels PACKAGE    # local/HTML dir of wheels
pip install --trusted-host my.index PACKAGE  # when TLS/certs are tricky

```

# Build control

```bash
pip install --only-binary=:all: PACKAGE      # wheels only
pip install --no-binary=:all: PACKAGE        # force build from source
pip install --no-build-isolation PACKAGE     # use current env to build

```

# Environments & locations

```bash
python -m venv .venv && source .venv/bin/activate
pip install --user PACKAGE                   # user site (no venv)
pip install --target vendor/ PACKAGE         # install into a folder

```

# Security / reproducibility

```bash
pip hash file.whl                            # compute secure hash
pip install --require-hashes -r req.txt      # pin with hashes

```

# Caching, debugging, config

```bash
pip cache dir            # show cache location
pip cache purge          # clear cache
pip debug                # env and compile flags info
pip config list          # show pip config from files/env
pip inspect PACKAGE      # (newer) metadata view of a package archive

```

# VCS (install straight from Git, etc.)

```bash
pip install git+https://github.com/user/repo.git
pip install "git+https://github.com/user/repo.git@v1.2.3#egg=name"
pip install "git+ssh://git@github.com/user/repo.git"

```

# Requirements file tricks (put these inside `requirements.txt`)

```
--extra-index-url <https://my.index/simple>
--require-hashes
packageA==1.2.3 \\
    --hash=sha256:<...>
packageB>=2.0; python_version<"3.12"

```

# Jupyter/IPython tip

Inside notebooks, prefer:

```python
%pip install PACKAGE

```

(it installs into the **current kernel**)

## Quick gotchas

- Prefer `python -m pip ...` over `pip ...` when multiple Pythons exist.
- Use **virtual environments** to avoid system pollution.
- If wheels aren’t available, `pip` may compile from source—install system build tools if needed.
- Pre-releases are skipped unless you pin them or use `-pre`.

If you share your use case (dev workflow, CI, private index, etc.), I’ll tailor a few ready-to-paste command sets and a sample `requirements.txt`/`constraints.txt` for you.