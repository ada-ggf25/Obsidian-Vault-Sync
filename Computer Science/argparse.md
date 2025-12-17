#computer_science #python #python_libraries

[[argpase Vs sys.argv]]

Awesome—here’s a practical-but-deep guide to **`argparse`** so you can design solid CLIs fast.

# 1) Core idea

`argparse` turns command-line strings into a Python object (**Namespace**). You declare **positional** and **optional** arguments, then call `parse_args()`.

```python
import argparse

p = argparse.ArgumentParser(
    prog="imgtool",
    description="Batch image processor",
    epilog="Examples: imgtool resize in/ out/ --width 512"
)
p.add_argument("src")                    # positional
p.add_argument("dst")
p.add_argument("-w","--width", type=int, default=256)   # optional
p.add_argument("-v","--verbose", action="count", default=0)
args = p.parse_args()

```

Run:

```
imgtool photos/ out/ --width 512 -vv

```

`args` → `Namespace(src='photos/', dst='out/', width=512, verbose=2)`

---

# 2) Argument types, choices, and validation

- `type=...` converts strings to Python values (`int`, `float`, `pathlib.Path`, custom callables).
- `choices=[...]` restricts values.
- Raise `argparse.ArgumentTypeError` from custom type functions for nice errors.

```python
from pathlib import Path
import argparse, re

def percent(s):
    m = re.fullmatch(r"(?:100|[0-9]{1,2})(?:\\.\\d+)?%", s)
    if not m:
        raise argparse.ArgumentTypeError("Expected number like 75%")
    return float(s[:-1]) / 100

p = argparse.ArgumentParser()
p.add_argument("--out", type=Path)
p.add_argument("--mode", choices=["fast","accurate"])
p.add_argument("--dropout", type=percent)

```

---

# 3) Positional vs optional, `nargs` and `metavar`

- **Positional**: required by position; **Optional**: start with or `-`.
- `nargs`: control arity:
    - `?` optional (0 or 1)
    - any (0+)
    - `+` one or more
    - integer `n` exact count
- `metavar="NAME"` controls placeholder text in help.

```python
p.add_argument("files", nargs="+", metavar="FILE")
p.add_argument("--scale", nargs=2, type=int, metavar=("W","H"))

```

---

# 4) Booleans and counters

```python
p.add_argument("-q","--quiet", action="store_true")  # False→True if present
p.add_argument("-v","--verbose", action="count", default=0)  # -v, -vv, -vvv

```

Other useful actions: `store_const`, `append`, `append_const`, `version`

```python
p.add_argument("--tag", action="append")  # --tag a --tag b → ["a","b"]
p.add_argument("--version", action="version", version="%(prog)s 1.2.0")

```

---

# 5) Subcommands (git-style CLIs)

Use **subparsers** to create `prog <command> [args]`.

```python
p = argparse.ArgumentParser(prog="imgtool")
sp = p.add_subparsers(dest="cmd", required=True)

resize = sp.add_parser("resize", help="Resize images")
resize.add_argument("src"); resize.add_argument("dst")
resize.add_argument("--width", type=int, required=True)

crop = sp.add_parser("crop", help="Crop images")
crop.add_argument("src"); crop.add_argument("dst")
crop.add_argument("--box", nargs=4, type=int, metavar=("L","T","R","B"))

args = p.parse_args()

if args.cmd == "resize":
    ...
elif args.cmd == "crop":
    ...

```

Pattern: set a **handler** on each subparser for cleaner dispatch:

```python
def do_resize(args): ...
resize.set_defaults(func=do_resize)

args = p.parse_args()
args.func(args)

```

---

# 6) Groups & mutual exclusion

Organize help and enforce incompatible options.

```python
# display group (for nice help sections)
g = p.add_argument_group("display options")
g.add_argument("--color", choices=["auto","always","never"], default="auto")

# mutually exclusive
mx = p.add_mutually_exclusive_group(required=False)
mx.add_argument("--dry-run", action="store_true")
mx.add_argument("--apply", action="store_true")

```

---

# 7) Defaults, env vars, and config files

- Default values via `default=...`.
- You can override defaults from **environment variables**:

```python
import os
p.add_argument("--api-key", default=os.getenv("IMG_API_KEY"))

```

- Or from **config files** with `set_defaults()`:

```python
cfg = {"width": 512}
p.set_defaults(**cfg)

```

- Accept arguments **from a file**: set `fromfile_prefix_chars="@"`

```python
p = argparse.ArgumentParser(fromfile_prefix_chars='@')
p.add_argument("--lr", type=float); p.add_argument("--epochs", type=int)

# params.txt contains:
# --lr 0.001
# --epochs 10
# usage:
#   train @params.txt

```

---

# 8) Parsing strategies

- `parse_args()` → errors out on unknown options.
- `parse_known_args()` → returns `(known, unknown)`; useful when you forward unknown flags to another tool.
- `parse_intermixed_args()` (3.7+): allows positionals and optionals to be interleaved more flexibly.

```python
known, rest = p.parse_known_args()

```

---

# 9) Formatting & UX polish

- **Formatter classes** tweak help output:

```python
argparse.ArgumentDefaultsHelpFormatter
argparse.RawDescriptionHelpFormatter
argparse.RawTextHelpFormatter

```

Combine with multiple inheritance:

```python
class RichFmt(argparse.ArgumentDefaultsHelpFormatter,
              argparse.RawDescriptionHelpFormatter):
    pass

p = argparse.ArgumentParser(formatter_class=RichFmt, description=__doc__)

```

- Customize usage/prog:

```python
p = argparse.ArgumentParser(prog="imgtool", usage="%(prog)s <cmd> [options]")

```

- Suppress help for advanced flags:

```python
p.add_argument("--debug", help=argparse.SUPPRESS)

```

---

# 10) Custom actions (advanced)

Implement nonstandard behaviors (e.g., parse `KEY=VAL` pairs).

```python
class KVAction(argparse.Action):
    def __call__(self, parser, ns, values, option_string=None):
        d = getattr(ns, self.dest) or {}
        for v in values:
            k, eq, val = v.partition("=")
            if not eq:
                raise argparse.ArgumentTypeError("Use KEY=VALUE")
            d[k] = val
        setattr(ns, self.dest, d)

p.add_argument("--set", nargs="+", action=KVAction, default={})
# --set a=1 b=2 → args.set == {"a":"1","b":"2"}

```

---

# 11) Parents, prefixes, abbreviations

- **Parents**: reuse common options across parsers:

```python
common = argparse.ArgumentParser(add_help=False)
common.add_argument("-v","--verbose", action="count", default=0)

p = argparse.ArgumentParser(parents=[common])
sub = p.add_subparsers(dest="cmd", required=True)
build = sub.add_parser("build", parents=[common])

```

- **prefix_chars**: change to something else (rare).
- **allow_abbrev** (True by default): `-ver` matches `-version`. Turn off for strict CLIs:

```python
p = argparse.ArgumentParser(allow_abbrev=False)

```

---

# 12) Error handling and exit behavior

- On user error, `argparse` prints a message and exits with code **2**.
- Override `error()` for custom behavior:

```python
class Parser(argparse.ArgumentParser):
    def error(self, msg):
        raise SystemExit(f"error: {msg}\\nUse -h/--help for usage.")

```

- For library code, prefer `parse_known_args()` and handle errors yourself.

---

# 13) Testing your parser

Use `parse_args` with a list to simulate CLI input:

```python
def test_cli():
    p = make_parser()
    ns = p.parse_args(["resize","in","out","--width","640"])
    assert ns.width == 640

```

---

# 14) Common design patterns

**Log level flags**

```python
p.add_argument("-q","--quiet", action="store_true")
p.add_argument("-v","--verbose", action="count", default=0)

def setup_logging(args):
    level = {0:"WARNING",1:"INFO",2:"DEBUG"}.get(args.verbose, "DEBUG")

```

**Output to file or stdout**

```python
import sys
p.add_argument("-o","--output", type=argparse.FileType("w"),
               default=sys.stdout)
# use args.output.write(...)

```

**Path handling with `pathlib`**

```python
from pathlib import Path
p.add_argument("--in-dir", type=Path)

```

**Mapping to dataclasses**

```python
from dataclasses import dataclass

@dataclass
class Config:
    src: str
    dst: str
    width: int = 256

cfg = Config(**vars(p.parse_args()))

```

---

# 15) `argparse` vs Click/Typer (when to choose)

- **argparse**: stdlib, no deps, great for many tools, excellent subcommand support.
- **Click/Typer**: decorator-based, more ergonomic for large CLIs, automatic shell completion, richer prompts. Trade-off: extra dependency.

---

## Minimal, production-ready skeleton

```python
#!/usr/bin/env python3
"""imgtool — tiny example CLI."""

from __future__ import annotations
import argparse, sys
from pathlib import Path

def build_parser() -> argparse.ArgumentParser:
    class Fmt(argparse.ArgumentDefaultsHelpFormatter,
              argparse.RawDescriptionHelpFormatter):
        pass

    p = argparse.ArgumentParser(
        prog="imgtool",
        description=__doc__,
        formatter_class=Fmt,
    )
    p.add_argument("-v","--verbose", action="count", default=0,
                   help="Increase verbosity (-v, -vv)")
    sp = p.add_subparsers(dest="cmd", required=True)

    p_resize = sp.add_parser("resize", help="Resize images", formatter_class=Fmt)
    p_resize.add_argument("src", type=Path)
    p_resize.add_argument("dst", type=Path)
    p_resize.add_argument("--width", type=int, required=True)
    p_resize.set_defaults(func=cmd_resize)

    return p

def cmd_resize(args):
    if not args.src.exists():
        sys.exit(f"error: {args.src} not found")
    print(f"Resizing {args.src} → {args.dst} to width={args.width}")

def main(argv=None):
    p = build_parser()
    args = p.parse_args(argv)
    return args.func(args)

if __name__ == "__main__":
    raise SystemExit(main())

```

---

If you tell me the CLI you’re building (commands, flags, expected inputs), I’ll draft the exact `argparse` parser + handlers tailored to it, including tests and a `__main__.py` entry point.

---