#computer_science #terminal

`echo $PATH` prints the value of your shell’s **PATH environment variable**.

## What is `PATH`?

`PATH` is a **colon-separated list of directories** the shell searches (in order) to find a command you type.

Example:

```
/usr/local/bin:/usr/bin:/bin:/home/you/.local/bin
```

When you run `python`, the shell looks for an executable named `python` in `/usr/local/bin`, then `/usr/bin`, etc., stopping at the first match.

## Why `echo $PATH`?

- **Debug which folders are searched** for executables.
    
- **Verify installs** (e.g., did `/home/you/.local/bin` get added?).
    
- **Check order** (earlier entries win).
    

## Handy related commands

```bash
# Show each entry on its own line
echo "$PATH" | tr ':' '\n'

# See which executable will run
which python
# or (more reliable across shells)
command -v python

# Temporarily prepend a dir for this shell session
export PATH="/opt/mytool/bin:$PATH"

# Make it permanent (bash)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## On other shells/OS

- **zsh:** same as bash.
    
- **Fish:** `echo $PATH` shows entries on separate lines; set with `set -Ux PATH /new/dir $PATH`.
    
- **PowerShell (Windows):** `echo $env:Path`
    
- **cmd.exe (Windows):** `echo %PATH%`
    

### Quick tip

If a command isn’t found but you just installed it, either add its install directory to `PATH` (prepend it) or reopen the terminal so updated PATH is loaded.

It’s your **PATH**—the ordered list of folders WSL searches to find commands. Earlier entries win. Let’s decode yours:

```
/home/guilherme/miniconda3/bin
/home/guilherme/miniconda3/condabin
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
/usr/games:/usr/local/games
/usr/lib/wsl/lib
/mnt/c/Program Files/WindowsApps/...WindowsSubsystemForLinux...
/mnt/c/Windows/system32
/mnt/c/Windows
/mnt/c/Windows/System32/Wbem
/mnt/c/Windows/System32/WindowsPowerShell/v1.0
/mnt/c/Windows/System32/OpenSSH
/mnt/c/Program Files/dotnet
/mnt/c/Program Files/Docker/Docker/resources/bin
/mnt/c/Program Files/cursor/resources/app/bin
/mnt/c/Program Files/Git/cmd
/mnt/c/Program Files/GitHub CLI
/mnt/c/Users/guilh/AppData/Local/Microsoft/WindowsApps
/mnt/c/Users/guilh/AppData/Local/Programs/Microsoft VS Code/bin
/snap/bin
```

### What this tells us

- **`(base)`** means your **Conda base env is active**. So `python`/`pip` resolve to:
    
    - `/home/guilherme/miniconda3/bin/python`
        
    - `/home/guilherme/miniconda3/bin/pip`
        
- Standard Linux dirs follow (`/usr/bin`, etc.).
    
- Then a bunch of **Windows paths** under `/mnt/c/...`. WSL can run Windows EXEs; those entries let you call things like `code`, `pwsh`, `dotnet`, etc., from inside WSL.
    
- You also have Windows Git/gh on PATH (`/mnt/c/Program Files/Git/cmd`, `/mnt/c/Program Files/GitHub CLI`). Since `/usr/bin` appears **before** them, WSL will prefer **Linux git/gh** if installed—which is good (less cross-OS weirdness).
    

### Quick checks

```bash
which python
which pip
which git
which gh
echo "$PATH" | tr ':' '\n'   # pretty-print PATH entries
```

### Common actions

- **Use system Python instead of conda (temporarily):**
    
    ```bash
    conda deactivate
    ```
    
- **Ensure a directory wins:** prepend it for this session
    
    ```bash
    export PATH="$HOME/.local/bin:$PATH"
    ```
    
- **Make PATH changes permanent (bash):**
    
    ```bash
    echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
    source ~/.bashrc
    ```
    
- **Avoid mixing Windows tools by accident:** since `/usr/bin` is earlier, you’re fine. If you ever see `/mnt/c/.../Git/cmd` first, move it **after** Linux dirs or uninstall the Windows PATH export in WSL.
    

### TL;DR

Your shell will use **Conda’s Python/pip** first, then standard Linux tools, and only then fall back to Windows executables via WSL interop. If a command “isn’t the one you expect,” adjust the order or deactivate Conda as needed.

---

No worries—let’s make it super simple.

## The idea

- Your computer has many **folders** that contain programs.
    
- **PATH** is just a **list of those folders**.
    
- When you type a command (like `python`), the shell looks **through the PATH folders in order** and runs the **first match** it finds.
    

`echo $PATH` just **prints that list** so you can see where the shell is looking.

## Tiny story

You type: `python`  
The shell does:

1. Look in folder #1 from PATH. If `python` is there → run it.
    
2. If not, look in folder #2.
    
3. Keep going until found (or “command not found”).
    

## Why you care

- If a program isn’t found → its folder probably isn’t in PATH.
    
- If the **wrong version** runs → a different folder appears **earlier** in PATH.
    

## Quick hands-on (WSL/Linux)

Copy–paste these to make it click:

```bash
# 1) See your search folders
echo "$PATH" | tr ':' '\n'

# 2) See exactly which python will run
command -v python

# 3) Pretend we add a fake "bin" first in the search order
mkdir -p ~/demo/bin
echo -e '#!/usr/bin/env bash\necho HI_FROM_DEMO' > ~/demo/bin/python
chmod +x ~/demo/bin/python

# Put that folder FIRST, just for this shell
export PATH="$HOME/demo/bin:$PATH"

# Now which python runs?
command -v python    # -> /home/you/demo/bin/python
python               # -> prints HI_FROM_DEMO
```

You just changed which `python` runs by moving a folder to the front of PATH.  
That’s all PATH is: **the search order**.

## Cheats

- Show PATH neatly: `echo "$PATH" | tr ':' '\n'`
    
- Which command will run: `command -v <name>`
    
- Temporarily put a folder first: `export PATH="/my/folder:$PATH"`
    
- Make permanent (bash): add that export line to `~/.bashrc`, then `source ~/.bashrc`.
    

If you want, paste your `command -v python` and `echo "$PATH" | tr ':' '\n'` outputs and I’ll point to exactly which entry is winning.