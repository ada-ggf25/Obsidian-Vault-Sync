#computer_theory #computer_theory #python 

In `%prun`/`cProfile` output, the **ncalls** column shows `X/Y` when a function is **recursive**.

- `X` = **total calls** (including recursive re-entries)
- `Y` = **primitive calls** (calls that were **not** triggered by a recursive call to the same function)

So a line like `131/127` means the function was entered **131** times in total, of which **127** were primitive (4 were additional recursive re-entries).

Tip: the two “percall” columns use these counts differently:

- left `percall` = `tottime / total calls (X)`
- right `percall` = `cumtime / primitive calls (Y)`