#computer_science #python 

Totally doable! Here are the practical ways to use **pdb in Jupyter notebooks**.

# 1) Break anywhere with `breakpoint()`

```python
def foo(n):
    s = 0
    for i in range(n):
        s += i
        if i == 3:
            breakpoint()   # drops into pdb here
    return s
foo(6)  # at (Pdb) use: l, p i, p s, n, c, q

```

- When you hit `breakpoint()`, the cell pauses and shows the `(Pdb)` prompt.
- Type `c` (continue) to let the cell finish and return control to the kernel.

# 2) Auto-open pdb on exceptions (`%pdb on`)

```python
%pdb on     # enable once per kernel

```

Now any unhandled exception will drop you into pdb at the crash site.

Turn off with `%pdb off`.

# 3) Post-mortem on the last error (`%debug`)

If a cell just errored:

```python
%debug

```

You’ll land in pdb in the failing frame. Use normal pdb commands (`l, w, p, n, s, c, q`).

# 4) Run a script under the debugger from a notebook (`%run -d`)

Useful when your heavy logic is in a `.py`:

```python
%run -d myscript.py arg1 arg2
# At the (Pdb) prompt inside the notebook terminal:
#   b 42              # set breakpoint at line 42 of myscript.py
#   c                 # continue to it

```

Tip: `b func_name` sets a breakpoint on a function, `l` lists source, `q` quits.

# 5) Conditional/one-shot breaks in notebooks

```python
for i in range(10_000):
    if i % 997 == 0:
        breakpoint()  # or guard with if-condition yourself

```

(There’s no inline `condition` command for `breakpoint()`—just wrap it in `if`.)

# 6) Nice workflow patterns

- Put `breakpoint()` _inside_ the function you’re testing rather than at the call site.
- Use `%pdb on` during exploratory work; turn it off when you’re done.
- Pair with `%xmode verbose` to get richer tracebacks before `%debug`.

# What to expect / limitations

- You can’t “step to the next **cell**”; pdb steps through _code_, not notebook cells.
- `c` resumes execution of the _current cell_ until the next breakpoint/exception.
- Output appears inline; if the UI looks cramped, widen your notebook or use an external terminal for long sessions (`%run -d` on scripts).

# Minimal reproducible demo (copy/paste)

```python
%xmode verbose
%pdb on

def div(a, b):
    return a / b

def pipeline(x):
    y = x + 1
    breakpoint()  # stop here
    return div(y, 0)  # this will trigger post-mortem if not caught

pipeline(3)

```

Try: at `(Pdb)` run `l`, `a`, `p x`, `n`, `s`, `c`; after the ZeroDivisionError, run `%debug`.

If you want, tell me your exact bug (loop, numpy op, or test) and I’ll sketch the most efficient notebook-friendly pdb flow for it.