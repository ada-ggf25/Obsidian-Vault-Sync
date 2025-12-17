#python #computer_science #new #new_tools 

Great question — they do very different jobs:

# One-liner differences

- **mypy / Pyright** → **static type checkers**. Analyze your code _without running it_ to find type errors early.
- **Ruff** → **linter/formatter/import sorter**. Enforces style, catches many code smells, auto-fixes formatting/imports. **Not** a type checker.
- **Pydantic** → **runtime data validation & parsing**. Uses type hints to **validate/convert data at runtime** (e.g., JSON → Python objects).

# What each catches/does

|Tool|Category|Catches/examples|Runs when|
|---|---|---|---|
|**Pyright / mypy**|Static types|Passing `str` to a function that expects `int`; wrong return type; unreachable paths|During dev/CI (no program execution)|
|**Ruff**|Lint/format|Unused imports/vars, shadowing, bad complexity, common bugs (e.g., mutable default), formatting, import order|During dev/CI; can auto-fix|
|**Pydantic**|Runtime validation|Ensures incoming data matches a schema; converts `"123"` → `int`; raises clear errors on bad input|When your code runs|

# Tiny examples

## Static checker (Pyright/mypy) finds this:

```python
def add(a: int, b: int) -> int:
    return a + b

add("1", 2)  # ❌ Pyright/mypy: Argument 1 to "add" has incompatible type "str"

```

Ruff won’t flag this; it’s not a style issue.

## Ruff catches this:

```python
def f(x):
    y = 3
    return x
    print(y)   # ❌ Ruff: unreachable code; also might flag unused variables/imports

```

Type checkers may not warn about style/unreachable unless configured; Ruff is great here.

## Pydantic validates at runtime:

```python
from pydantic import BaseModel

class User(BaseModel):
    id: int
    email: str

u = User.model_validate({"id": "123", "email": "a@b.com"})
assert u.id == 123              # ✅ coerced
User.model_validate({"id": "x", "email": "a@b.com"})  # ❌ ValidationError at runtime

```

Static checkers can’t stop bad external data; Pydantic does.

# How they fit together in a project

- **Use Ruff** in pre-commit/CI to keep code clean & auto-formatted.
- **Use Pyright (editor) or mypy (CI)** to prevent type bugs before runtime. (Many teams use Pyright in editor for speed; optional mypy in CI.)
- **Use Pydantic** at API boundaries: request bodies, env vars, config, DB/JSON payloads.

# Config placement (pyproject.toml snippets)

```toml
[tool.ruff]
line-length = 100
target-version = "py311"
lint.select = ["E","F","I","B","UP","RUF","PL"]

[tool.mypy]
python_version = "3.11"
strict = true
ignore_missing_imports = true

```

Run:

```bash
# Ruff
ruff check . && ruff format .

# Pyright (or mypy)
pyright
# or
mypy src

```

# TL;DR

- **Ruff** = code hygiene & formatting.
- **Pyright/mypy** = prevent type mistakes at dev time.
- **Pydantic** = guarantee data correctness at runtime.

They’re complementary; using all three gives you clean, correct, and robust Python.