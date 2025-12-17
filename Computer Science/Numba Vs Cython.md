#python #computer_theory #computer_science 

Short answer: **yes**—you can use Numba and Cython **in the same project**, but you typically don’t mix them inside the _same_ function.

### Practical ways to combine them

- **Split responsibilities**
    - Use **Cython** to wrap C/C++ libraries or to hand-optimize complex code that needs AOT builds, `nogil`, and fine control over types/memory.
    - Use **Numba** to JIT-accelerate pure-Python numeric kernels and loops that are hard to vectorize.
- **Call one from the other (via C ABI)**
    - From **Cython → Numba**: create a C-callable Numba function and pass its address to Cython.
        
        ```python
        # numba_side.py
        from numba import cfunc, types
        
        @cfunc(types.float64(types.float64, types.float64))
        def add_nb(a, b):
            return a + b
        
        addr = add_nb.address  # integer address of C function
        
        ```
        
        ```
        # cython_side.pyx
        ctypedef double (*binop_t)(double, double)
        
        cdef double call_add(long long addr, double a, double b):
            cdef binop_t f = <binop_t>addr
            return f(a, b)
        
        ```
        
    - From **Numba → Cython**: expose a **Cython cdef** function with a C signature (optionally `nogil`), then get a C function pointer (via `ctypes`/`cffi`) and call it from Numba using `ctypes` in `nopython` mode (Numba supports calling external C functions via `ctypes` if the signature is known).
        

### Things to avoid/know

- You **can’t** decorate a Cython function with `@njit` and expect Numba to compile it; compiled Cython functions aren’t plain Python callables.
- An `@njit` function **can’t call** arbitrary Python/Cython functions unless they’re Numba-supported or you go through a C interface as above.
- Both Numba (`nopython`) and Cython (`nogil`) **release the GIL** in compiled sections, so either can parallelize safely there.
- Consider complexity: crossing boundaries (Numba↔Cython) adds build/JIT plumbing—do it only where it clearly pays off.

**Rule of thumb:** use each where it’s strongest; only wire them together via a C interface when you have a clear, measured need.