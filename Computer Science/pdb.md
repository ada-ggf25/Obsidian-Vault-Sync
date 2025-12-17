#computer_science #python 

[[pdb - Cheat Cheet]]
[[pdb in Notebooks]]

Here’s a fast, practical guide to `pdb` so you can start debugging right away.

# Quick start

**Option A: run your script under the debugger**

```bash
python -m pdb your_script.py arg1 arg2

```

You’ll drop into `(Pdb)` before the first line. Type `c` to run until a breakpoint.

**Option B: break only where you need**

```python
def foo(x):
    y = x + 1
    breakpoint()            # Python 3.7+ (uses pdb by default)
    return y * 2

```

or

```python
import pdb; pdb.set_trace()

```

Run normally: `python your_script.py` — execution will pause at that line.

# The 90% of commands you’ll use

- **l** – list source around the current line
- **n** – next line (step over)
- **s** – step into the current call
- **c** – continue until next breakpoint
- **r** – run until current function **r**eturns
- **u / d** – move **u**p / **d**own the stack
- **w** – where (stack trace)
- **p expr** – print expression (e.g., `p x.shape`)
- **pp expr** – pretty-print (great for dicts)
- **! stmt** – execute Python (mutate state, call functions, etc.)
- **q** – quit

# Breakpoints (power features)

- **b** – list breakpoints
- **b [file.py:123](http://file.py:123)** – set breakpoint at a line
- **b func_name** – break on function entry
- **tbreak [file.py:123](http://file.py:123)** – one-shot breakpoint
- **condition 1 x > 10** – make breakpoint #1 conditional
- **ignore 1 5** – skip first 5 hits of breakpoint #1
- **disable 1 / enable 1 / clear 1** – manage a breakpoint

# Inspecting state

- **a** – show function arguments
- **p/pp var** – evaluate variables/expressions
- **display expr** – auto-print an expression on every step/stop
- **undisplay 1** – remove a display #1

# Post-mortem (when an exception already happened)

From a failing run that produced a traceback:

```bash
python -m pdb your_script.py
# or:
python -m pdb -c continue your_script.py

```

Pdb will break at the exception site when it occurs.

Inside code:

```python
try:
    risky()
except Exception:
    import pdb, sys, traceback
    traceback.print_exc()
    pdb.post_mortem()   # open pdb at the crash frame

```

# In IPython/Jupyter

- **%pdb on** – auto-open pdb on exceptions
- **breakpoint()** works in cells too. Use **c** to keep running the notebook kernel until next stop.

# Tips for real projects

- Put `breakpoint()` where the bug manifests (right before a suspicious branch/loop).
- Use **s** (step into) only when needed; prefer **n**/**r** to move faster.
- Combine with logging: use `!print(...)` or `p` to inspect without editing code.
- Avoid `j` (jump to line) unless you really know control-flow implications (it skips code execution).

# Minimal example to practice

```python
def f(n):
    total = 0
    for i in range(n):
        total += i
        if i == 3:
            breakpoint()
    return total

if __name__ == "__main__":
    print(f(6))

```

At the `(Pdb)` prompt try: `l`, `a`, `p i`, `p total`, `n`, `s`, `r`, `c`.

If you want, tell me what you’re debugging (script, tests, or notebook) and I’ll suggest an exact workflow (e.g., pytest + pdb, conditional breaks for NumPy loops, etc.).