#computer_science #git #github 

`git pull --ff-only` = **fetch + fast-forward only**.

- **What it does:** downloads the remote branch and **updates your local branch only if it can move the pointer forward** without creating a merge commit.
    
- **If your branch has no unique commits** (you’re strictly behind remote), it fast-forwards:
    
    ```
    A—B—C   origin/main
    A—B     main           git pull --ff-only  ➜  main → A—B—C
    ```
    
- **If you have local commits** that aren’t on the remote (history has diverged), it **aborts** with an error instead of auto-merging:
    
    ```
    A—B—C   origin/main
    A—B—D   main           (diverged)  ➜  pull --ff-only fails
    ```
    

### Why use it

- Keeps history **linear** (no unintended merge commits).
    
- Acts as a **safety check** so you notice divergence and decide explicitly how to proceed (merge or rebase).
    

### When it fails (what to do)

If it refuses to fast-forward:

```bash
# See what changed remotely
git fetch origin
git log --oneline --decorate --graph --boundary origin/MAIN..HEAD

# Option 1: rebase your commits on top of remote
git rebase origin/MAIN
# then update remote
git push --force-with-lease

# Option 2: merge explicitly (creates a merge commit)
git merge origin/MAIN
git push
```

(Replace `MAIN` with your branch name, e.g., `main` or `master`.)

### Variants & settings

- Pull a specific remote/branch:
    
    ```bash
    git pull --ff-only origin main
    ```
    
- Make it the default:
    
    ```bash
    git config --global pull.ff only
    # or per-repo:
    git config pull.ff only
    ```
    
- Compare:
    
    - `git pull` (default) = fetch + **merge** (may create a merge commit).
        
    - `git pull --rebase` = fetch + **rebase** your local commits on top of remote.
        
    - `git pull --ff-only` = **refuse** if anything but a fast-forward is needed.