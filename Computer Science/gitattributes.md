#git #computer_science 

A **.gitattributes** file tells Git how to treat _paths_ in your repo. It’s committed (lives at the repo root or in subfolders) and applies attributes by pattern.

## What you use it for

- **Line endings / cross-OS**
    
    Normalize text files and avoid CRLF/LF churn.
    
- **Mark files as binary or text**
    
    Controls diffs, merges, and end-of-line normalization.
    
- **Custom diff/merge drivers**
    
    Use specialized tools for e.g. Jupyter notebooks, images, lockfiles.
    
- **Git LFS**
    
    Route large/binary files to Git LFS.
    
- **Archive/export behavior**
    
    Exclude files from `git archive` (`export-ignore`).
    
- **Filters**
    
    Smudge/clean transforms (e.g., keyword expansion, encryption).
    

## Syntax basics

Each line: `<pattern> <attr> [<attr> ...]`

Patterns are like `.gitignore` (globs). Attributes are boolean or key/value.

```
# Line endings (recommended)
* text=auto                     # detect text, normalize to LF in repo

# Force specific types
*.sh        text eol=lf
*.bat       text eol=crlf
*.png       binary               # no EOL conversion or diffs
*.pdf       binary

# Git LFS
*.psd       filter=lfs diff=lfs merge=lfs -text
models/**   filter=lfs diff=lfs merge=lfs -text

# Custom diff/merge (example: Jupyter notebooks)
*.ipynb     diff=ipynb
# then define the driver in .git/config or .gitconfig:
# [diff "ipynb"]
#   textconv = jupyter nbconvert --to=markdown --stdout

# Lock files: prefer ours on merge to reduce noise
package-lock.json merge=ours
poetry.lock      merge=ours

# Archives: ignore
docs/_build/**   export-ignore

```

## Common presets

**Cross-platform codebase**

```
* text=auto
*.py text eol=lf
*.md text eol=lf
*.bat text eol=crlf
*.png binary
*.jpg binary

```

**Data / models with LFS**

```
data/**/*      filter=lfs diff=lfs merge=lfs -text
checkpoints/** filter=lfs diff=lfs merge=lfs -text

```

**Notebooks**

```
*.ipynb text -diff       # or use a custom diff driver as above

```

## Gotchas & tips

- `text=auto` + per-extension overrides is the simplest portable setup.
    
- Changing attributes **doesn’t rewrite history**; to re-normalize line endings:
    
    ```bash
    git rm --cached -r .
    git reset --hard
    
    ```
    
- `binary` implies `diff -text` (no EOL normalization, no textual diff).
    
- Attributes in subdirectory `.gitattributes` files override higher levels.
    
- For custom drivers/filters you must also configure them in Git config.
    
- For LFS you must `git lfs install` and **commit the .gitattributes** before adding files.
    

If you tell me your stack (OS mix, presence of notebooks, large data, etc.), I’ll draft a tailored `.gitattributes` you can drop into your repo.