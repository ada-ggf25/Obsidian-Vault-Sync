#python #computer_theory #python_libraries 

Great question! That pattern makes your code safer, reusable, and easier to test.

# Why `if __name__ == "__main__": main()`

- **Prevents side effects on import.**
    
    When a file is imported, `__name__` is the module’s name (e.g., `"calc"`), not `"__main__"`.
    
    So the block **doesn’t run** during `import calc`, avoiding surprise executions (CLI parsing, network calls, etc.).
    
- **Lets one file be both a library and a script.**
    
    You can `from calc import add` inside other programs, _and_ still run `python calc.py` as a CLI.
    
- **Essential on Windows multiprocessing.**
    
    The spawn method re-imports the module in child processes; the guard stops the top-level code from running again.
    
- **Single, explicit entry point.**
    
    Reviewers and tools know “program starts here,” which helps readability and profiling.
    

# Why put logic inside `main()`

- **Testability.**
    
    You can call `main()` (or better, a `run(args)` function) directly in unit tests with fake arguments—no subprocess needed.
    
    ```python
    def run(args): ...
    def main(argv=None):
        args = parser().parse_args(argv)
        return run(args)
    
    ```
    
    Then: `assert run(parsed_args) == 42` or `main(["-o","add","-n1","2","-n2","3"])`.
    
- **Separation of concerns.**
    
    Top-level defines functions/classes; `main()` handles wiring: parse args, configure logging, call workers, set exit code.
    
- **No global state / cleaner namespace.**
    
    Variables inside `main()` don’t leak as module-level globals. Fewer surprises, easier refactors.
    
- **Clear exit codes.**
    
    `raise SystemExit(code)` (or `return code` from `main()` and `sys.exit(main())`) integrates with shells/CI.
    
- **Startup cost is delayed.**
    
    Heavy imports or config can live in `main()` so importing the module is cheap.
    
- **Packaging-friendly.**
    
    Easy to expose as a console script:
    
    ```toml
    [project.scripts]
    calc = "calc:main"
    
    ```
    

# Minimal recommended skeleton

```python
import argparse
import sys

def parser():
    p = argparse.ArgumentParser(description="Arithmetic CLI")
    p.add_argument("-o","--operation", choices=["add","sub","mul","div"], required=True)
    p.add_argument("-n1", type=float, required=True)
    p.add_argument("-n2", type=float, required=True)
    return p

def run(op, n1, n2):
    if op == "add": return n1 + n2
    if op == "sub": return n1 - n2
    if op == "mul": return n1 * n2
    if op == "div":
        if n2 == 0: raise ZeroDivisionError("division by zero")
        return n1 / n2

def main(argv=None) -> int:
    args = parser().parse_args(argv)
    try:
        result = run(args.operation, args.n1, args.n2)
        print(result)
        return 0
    except Exception as e:
        print(f"error: {e}", file=sys.stderr)
        return 1

if __name__ == "__main__":
    raise SystemExit(main())

```

**TL;DR:** the `if __name__ == "__main__": main()` guard + a `main()` function makes your module import-safe, reusable as a library, easy to test, packaging-ready, and robust with multiprocessing—hallmarks of professional Python code.