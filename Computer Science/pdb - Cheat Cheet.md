#computer_science #python 

Here’s a compact **pdb cheat sheet** you can keep handy.

# Start the debugger

- Run script under pdb: `python -m pdb script.py [args]`
- Break in code: `breakpoint()` (Py3.7+) or `import pdb; pdb.set_trace()`

# Execution control

- `c` – **c**ontinue until next breakpoint
- `n` – step to **n**ext line (step over)
- `s` – **s**tep into function
- `r` – **r**un until current function returns
- `unt <line>` – run **unt**il reaching line (same file)
- `j <line>` – **j**ump to line (skips code execution there—use with care)
- `q` – **q**uit

# Inspect code & stack

- `l` – **l**ist source around current line (repeat to scroll)
- `ll` – list entire function
- `w` – stack trace (“**w**here”)
- `u` / `d` – move **u**p / **d**own stack frames
- `a` – show function **a**rgs

# Evaluate & modify state

- `p <expr>` – **p**rint expression
- `pp <expr>` – pretty-print
- `! <stmt>` – execute Python statement (mutate state, call helpers)
- `whatis <name>` – show type / info
- `retval` – value just returned from a finished frame (after `r`)

# Breakpoints

- `b` – list breakpoints
    
- `b <file.py>:<line>` – set breakpoint at file/line
    
    (shorthand: `b <line>` for current file)
    
- `b <func>` – break on function entry
    
- `tbreak <loc>` – one-shot breakpoint
    
- `condition <num> <expr>` – make breakpoint #**num** conditional
    
- `ignore <num> <count>` – ignore first N hits of breakpoint
    
- `disable <num>` / `enable <num>` – toggle
    
- `clear <num>` or `clear` – delete one / all
    

# Watches & auto-display

- `display <expr>` – auto-print expr at every stop
- `undisplay <num>` – remove a display
- `until <line>` – (alias of `unt`) handy to run to loop end

# Exceptions & post-mortem

- Run and stop on crash: `python -m pdb script.py` (it will break at the exception)
    
- From code after catching:
    
    ```python
    import pdb, traceback; traceback.print_exc(); pdb.post_mortem()
    
    ```
    
- Start, then auto-continue: `python -m pdb -c continue script.py`
    

# Navigation & search

- `help` / `h <cmd>` – built-in help
- `where` – alias of `w`
- `alias name command…` – create a command alias
- `unalias name` – remove it

# Quality-of-life tips

- Use `breakpoint()` near suspicious branches/loops.
- Prefer `n` and `r` to move quickly; use `s` only when needed.
- Keep `display var` for loop variables you’re tracking.
- Avoid `j` unless you understand skipped side effects.

# In IPython/Jupyter

- `%pdb on` – auto-open pdb on exceptions
- `breakpoint()` works in cells; use `c` to resume the kernel

# With pytest

- Failures drop into pdb: `pytest -q --pdb`
- Break anywhere: `pytest -q -k test_name --maxfail=1 -x` plus `breakpoint()` in code
- Step through test collection: `pytest --pdb --pdbcls=IPython.terminal.debugger:TerminalPdb` (nicer UI)

---

Want this as a one-page printable PDF? Say “make it a PDF” and I’ll format it for printing.