#computer_science #new_tools #virtual_environment

[[Conda Vs Mamba]]

**Conda** is a **cross-platform package manager and environment manager**.

- **Package manager:** installs prebuilt packages (Python, R, C/C++ libs, CUDA, MKL, GDAL, etc.) from “**channels**” like `defaults` or `conda-forge`.
    
- **Environment manager:** creates **isolated environments**, each with its **own Python version** and packages, so projects don’t clash.
    
- **Reproducible:** environments can be exported to `environment.yml` and rebuilt elsewhere.
    
- **Cross-language:** not just Python—can manage tools and native libs, which pip alone can’t.
    

### Mental model

Conda = `apt + virtualenv` for data/ML: it installs system-level deps **and** keeps them sandboxed per project.

### Common commands

```bash
# Update conda itself
conda update -n base -c defaults conda

# New env with its own Python
conda create -n myenv python=3.11
conda activate myenv

# Install packages (conda-forge is popular)
conda install -c conda-forge numpy pandas

# Mix in pip if needed (inside the env)
pip install package-not-on-conda

# List / export / remove
conda env list
conda list
conda env export > environment.yml
conda remove -n myenv --all
```

### Variants

- **Miniconda/Anaconda/Mini/Mambaforge** are just **distributions that ship conda** (Mambaforge includes **mamba**, a faster solver compatible with conda).
    

If you tell me your stack (e.g., PyTorch + CUDA on WSL, or pure-Python web), I can give you a ready-to-paste env spec.