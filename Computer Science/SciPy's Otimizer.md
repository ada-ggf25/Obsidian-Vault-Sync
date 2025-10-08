#computer_theory #computer_science #math #python 

Totally—this is just showing how to use **SciPy’s optimizer** to find minima of functions.

### 1) 1-D example (`minimize_scalar`)

```python
def f(x):
    return -np.exp(-(x - 0.2)**4)

result = sopt.minimize_scalar(f)
x_min = result.x

```

- We give the optimizer a scalar function (f(x)).
- `minimize_scalar` searches for the **x** that **minimizes** (f).
- Your (f(x)) is always ≤ 0 and reaches its **lowest** value (-1) at (x=0.2) (because ((x-0.2)^4=0\Rightarrow e^0=1)).
- Output: `x_min ≈ 0.2`. That’s the point where the function is smallest.

### 2) 2-D example (`minimize`)

```python
def f(x):       # x is a vector [x0, x1]
    return .5*(1 - x[0])**2 + (x[1] - x[0]**2)**2    # Rosenbrock-like fn

def jacobian(x):   # gradient ∇f(x)
    return np.array((
        - (1 - x[0]) + 4*x[0]*(x[0]**2 - x[1]),
         2*(x[1] - x[0]**2)
    ))

sopt.minimize(f, [2, -1], method="Newton-CG", jac=jacobian)

```

- Here the variable is a **vector** (x=[x, y]).
- `minimize` is the general multivariate optimizer.
- We also pass the **gradient** (`jac=...`). Giving the gradient makes Newton/CG methods fast and accurate.
- Start from `[2, -1]` and the method iteratively updates the point to reduce `f`.
- The global minimum of this function is at ((x,y)=(1,1)) with value 0. The solver reports **success** when it converges.

### What “optimize” is doing under the hood

- It repeatedly evaluates your function (and optionally gradient/Hessian) and moves the parameters to **decrease** the function.
- Different algorithms = different rules for choosing the next step.
    - `minimize_scalar` → 1-D bracketing / Brent methods.
    - `minimize(..., method="Newton-CG")` → uses gradient (and Hessian–vector products) for fast convergence.
    - If you don’t have gradients, you can omit `jac` (it will estimate them numerically) or use methods like `"Nelder-Mead"`.

### When you’d use which

- **Unconstrained 1-D:** `minimize_scalar(f)`.
- **Unconstrained multi-D:** `minimize(f, x0, method="BFGS")` (no gradient needed) or `"CG"`, `"Newton-CG"` (with `jac`).
- **Bounds/constraints:** use `method="L-BFGS-B"` (bounds) or `"SLSQP"`/`"trust-constr"` (constraints).

If you want, give me a function you care about (e.g., a loss from a model), and I’ll show the exact `minimize` call and how to interpret the result object (`result.x`, `result.fun`, `result.nit`, etc.).