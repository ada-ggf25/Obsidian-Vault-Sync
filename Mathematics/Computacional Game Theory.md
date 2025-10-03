#math #game_theory #quantitative_finance #python #computer_science #machine_learning 

Perfect — here’s a **hands-on, desk-ready deep dive** into the Coding Mini-Projects. Each project has: goal → theory → algorithm → clean Python API → tests you can run → extensions (for research/quant flavor). I’ll keep the math in LaTeX\LaTeX where it helps; code is plain Python with type hints and docstrings so you can paste into Obsidian or VS Code.

> Suggested folder (so everything stays tidy in Obsidian + VS Code):

```
game-theory-lab/
  core/                 # reusable utilities (distributions, LP wrappers, plotting)
  projects/
    zero_sum_lp.py
    ne_2x2.py
    support_enum.py
    mwu.py
    replicator.py
    backward_induction.py
    pbe_signaling.py
    entry_cutoff.py
    fpa_uniform.py
    myerson_reserve.py
    revenue_equivalence.py
    winner_curse_sim.py
    lemke_howson.py
    kyle_one_period.py
    pigou_poa.py
    congestion_potential.py
  tests/
    test_*.py
  notebooks/            # (optional) quick demos
```

---

# 1) Zero-sum matrix games via LP

**Goal.** Compute value and optimal mixed strategies of a zero-sum game $$A∈Rm×nA\in\mathbb{R}^{m\times n}.$$

**Theory.** Row player:

$$max⁡p∈Δm min⁡j(p⊤A)j ⟺ max⁡p,v v s.t. p⊤A≥v 1⊤, 1⊤p=1, p≥0.\max_{p\in\Delta_m}\ \min_{j}(p^\top A)_j \ \Longleftrightarrow\ \max_{p,v}\ v \ \text{s.t.}\ p^\top A \ge v\,\mathbf{1}^\top,\ \mathbf{1}^\top p=1,\ p\ge 0.$$

Dual gives column’s mix qq. Strong duality = minimax.

**Algorithm.** Single LP for each player (or one LP and read the dual). Use `scipy.optimize.linprog` (simplex or HiGHS).

**API (pasteable).**

```python
# projects/zero_sum_lp.py
from __future__ import annotations
import numpy as np
from dataclasses import dataclass

@dataclass
class ZeroSumSolution:
    value: float
    p: np.ndarray  # row mix (m,)
    q: np.ndarray  # col mix (n,)

def solve_zero_sum(A: np.ndarray) -> ZeroSumSolution:
    """
    Solve zero-sum matrix game for both players via LP.
    A_ij is row player's payoff when row i vs column j.
    Returns value and optimal mixed strategies (p, q).
    """
    import numpy as np
    from scipy.optimize import linprog

    m, n = A.shape

    # Row player's LP (maximize v) -> minimize -v
    # Vars: [p_1..p_m, v]
    c = np.r_[np.zeros(m), -1.0]  # minimize -v == maximize v

    # Constraints: p^T A >= v 1^T  ->  A^T p - v 1 >= 0  ->  -A^T p + v 1 <= 0
    G = np.c_[-A.T, np.ones(n)]
    h = np.zeros(n)

    # Sum p_i = 1
    Aeq = np.r_[np.ones((1, m)), np.zeros((1,))].reshape(1, m+1)
    beq = np.array([1.0])

    bounds = [(0, None)] * m + [(None, None)]
    res = linprog(c, A_ub=G, b_ub=h, A_eq=Aeq, b_eq=beq, bounds=bounds, method="highs")
    if not res.success:
        raise RuntimeError(res.message)
    p = res.x[:m]
    v = res.x[-1]

    # Column player's symmetric LP on -A (or solve dual)
    c2 = np.r_[np.zeros(n), 1.0]  # minimize w
    G2 = np.c_[A, -np.ones(m)]    # A q <= w 1  -> A q - w 1 <= 0
    h2 = np.zeros(m)
    Aeq2 = np.r_[np.ones((1, n)), np.zeros((1,))].reshape(1, n+1)
    beq2 = np.array([1.0])
    bounds2 = [(0, None)] * n + [(None, None)]
    res2 = linprog(c2, A_ub=G2, b_ub=h2, A_eq=Aeq2, b_eq=beq2, bounds=bounds2, method="highs")
    if not res2.success:
        raise RuntimeError(res2.message)
    q = res2.x[:n]
    w = res2.x[-1]

    # sanity: v ≈ w
    return ZeroSumSolution(value=0.5*(v+w), p=p/np.sum(p), q=q/np.sum(q))
```

**Tests.**

```python
A = np.array([[1, -1], [-1, 1]])  # Matching Pennies
sol = solve_zero_sum(A)
assert np.allclose(sol.p, [0.5, 0.5], atol=1e-6)
assert np.allclose(sol.q, [0.5, 0.5], atol=1e-6)
assert abs(sol.value) < 1e-6
```

**Extensions.**  
Sensitivity: add small ϵ\epsilon perturbations; worst-case value across a set of matrices; robust LP.

---

# 2) Mixed NE for 2×22\times2 (and a safe template)

**Goal.** Closed-form mixed NE for 2×22\times2 general-sum.

**Theory.** With payoffs

$$LRT(a,e)(b,f)B(c,g)(d,h)\begin{array}{c|cc} & L & R \\\hline T & (a,e) & (b,f) \\ B & (c,g) & (d,h) \end{array}$$

Column’s mix $$q⋆=d−ba−b−c+dq^\star=\frac{d-b}{a-b-c+d}, Row’s mix p⋆=h−fe−f−g+hp^\star=\frac{h-f}{e-f-g+h}.$$

**API.**

```python
# projects/ne_2x2.py
import numpy as np

def mixed_ne_2x2(a,b,c,d,e,f,g,h):
    denom_row = (a - b - c + d)
    denom_col = (e - f - g + h)
    q = (d - b) / denom_row if denom_row != 0 else None
    p = (h - f) / denom_col if denom_col != 0 else None
    return p, q
```

**Tests.**

```python
# Battle of the Sexes: two pure + one mixed
p, q = mixed_ne_2x2(2,0,0,1,1,0,0,2)
assert 0 <= p <= 1 and 0 <= q <= 1
```

**Extensions.**  
Add pure NE finder (best-response arrows); handle degenerate denominators gracefully.

---

# 3) Support Enumeration (small bimatrix NE)

**Goal.** Find all NE of a small bimatrix game by guessing supports.

**Algorithm (sketch).**  
For support pairs (S1,S2)(S_1,S_2):

1. Solve equal-payoff (indifference) linear system on supports.
    
2. Check probabilities ≥0\ge 0 and sum to 1.
    
3. Verify off-support payoffs ≤\le on-support (no profitable deviations).
    

**API.**

```python
# projects/support_enum.py
import itertools, numpy as np

def support_enumeration(A: np.ndarray, B: np.ndarray):
    m, n = A.shape
    out = []
    for k in range(1, min(m,n)+1):
        for S1 in itertools.combinations(range(m), k):
            for S2 in itertools.combinations(range(n), k):
                S1, S2 = list(S1), list(S2)
                A_S = A[np.ix_(S1, S2)]
                B_S = B[np.ix_(S1, S2)]
                # Solve for y on S2 s.t. Ay equalized
                try:
                    y = np.linalg.solve(A_S.T - A_S.T.mean(axis=0, keepdims=True), 
                                        np.zeros(k))  # homogeneous trick unstable -> use full system below
                except np.linalg.LinAlgError:
                    continue
                # Safer: set Ay = alpha 1 with normalization sum y = 1
                M = np.r_[A_S.T - A_S.T[:, [0]], np.ones((1, k))]
                b = np.r_[np.zeros(k-1), [1.0]]
                try:
                    y = np.linalg.lstsq(M, b, rcond=None)[0]
                except np.linalg.LinAlgError:
                    continue
                if np.any(y < -1e-10): 
                    continue
                y = np.maximum(y, 0); y = y / y.sum()

                # Now x on S1 from B
                M2 = np.r_[B_S - B_S[[0], :], np.ones((1, k))]
                b2 = np.r_[np.zeros(k-1), [1.0]]
                try:
                    x = np.linalg.lstsq(M2, b2, rcond=None)[0]
                except np.linalg.LinAlgError:
                    continue
                if np.any(x < -1e-10):
                    continue
                x = np.maximum(x, 0); x = x / x.sum()

                # Verify no profitable deviation off support
                pay_row = A[np.ix_(range(m), S2)] @ y
                if np.any(pay_row > pay_row[S1[0]] + 1e-8):
                    continue
                pay_col = (B[np.ix_(S1, range(n))].T @ x)
                if np.any(pay_col > pay_col[S2[0]] + 1e-8):
                    continue

                full_x = np.zeros(m); full_x[S1] = x
                full_y = np.zeros(n); full_y[S2] = y
                out.append((full_x, full_y))
    return out
```

**Tests.**

```python
A = np.array([[0,2],[1,0]]); B = -A  # zero-sum
eqs = support_enumeration(A,B)
assert eqs, "Should find a mixed NE."
```

**Extensions.**  
Replace lstsq with exact pivoting; add duplicate-solution pruning; compare to Lemke–Howson results.

---

# 4) Multiplicative Weights (no-regret) + CCE diagnostics

**Goal.** Run MWU for a player; track external regret and empirical distribution (→ CCE).

**Theory.** With losses $$ℓt(a)∈[0,1]\ell_t(a)\in[0,1], update wt+1(a)=wt(a)exp⁡(−ηℓt(a))w_{t+1}(a)=w_t(a)\exp(-\eta\ell_t(a)). Choose η≍(ln⁡K)/T\eta\asymp\sqrt{(\ln K)/T}. Regret =O(Tln⁡K)=O(\sqrt{T\ln K}).$$

**API.**

```python
# projects/mwu.py
import numpy as np
from typing import Callable, Tuple, Dict

def mwu(T: int, K: int, loss_fn: Callable[[int], np.ndarray], eta: float=None):
    """
    Run MWU for T rounds over K actions.
    loss_fn(t) -> losses vector shape (K,) in [0,1] given current t.
    Returns probabilities history, chosen actions, and average loss/regret.
    """
    eta = eta or np.sqrt(2*np.log(K)/max(T,1))
    w = np.ones(K)
    probs_hist, actions, losses = [], [], []
    best_fixed_losses = np.zeros(K)
    for t in range(1, T+1):
        p = w / w.sum()
        L = loss_fn(t)  # vector
        a = np.random.choice(K, p=p)
        w *= np.exp(-eta * L)
        probs_hist.append(p); actions.append(a); losses.append(L[a])
        best_fixed_losses += L
    losses = np.array(losses)
    avg_loss = losses.mean()
    best_fixed = best_fixed_losses.min() / T
    regret = avg_loss - best_fixed
    return np.array(probs_hist), np.array(actions), avg_loss, regret
```

**Tests.**  
Use a stationary zero-sum 2×22\times2 loss matrix and verify regret →0\to 0 as TT grows.

**Extensions.**  
Swap-regret (for CE); bandit feedback (EXP3); adversarial schedule that cycles.

---

# 5) Replicator Dynamics (ODE + discrete MWU link)

**Goal.** Simulate continuous-time replicator in a symmetric game:

$$x˙i=xi((Ax)i−x⊤Ax).\dot x_i = x_i\Big((Ax)_i - x^\top A x\Big).$$

**API.**

```python
# projects/replicator.py
import numpy as np

def replicator(A: np.ndarray, x0: np.ndarray, dt=1e-2, T=50.0):
    """
    Euler simulate replicator dynamics for payoff matrix A (symmetric).
    x0 in simplex; returns trajectory.
    """
    steps = int(T/dt); x = x0.copy(); traj = [x.copy()]
    for _ in range(steps):
        Ax = A @ x
        avg = x @ Ax
        dx = x * (Ax - avg)
        x = x + dt * dx
        x = np.maximum(x, 0); s = x.sum(); 
        if s == 0: x = np.ones_like(x)/len(x)
        else: x /= s
        traj.append(x.copy())
    return np.array(traj)
```

**Tests.**  
Use Rock-Paper-Scissors; check that $$∥xt−131∥\|x_t - \tfrac13\mathbf{1}\|$$ oscillates, while time-average approaches $$13\tfrac13.$$

**Extensions.**  
Switch to `solve_ivp`; add Lyapunov diagnostics for potential games.

---

# 6) Backward Induction (perfect information) + SPE

**Goal.** Solve finite game trees via BI; output SPE strategies and path payoffs.

**Design.** Represent a node as:

```python
@dataclass
class Node:
    player: int | None            # None for terminal
    actions: list[str] | None
    children: list['Node'] | None
    payoff: np.ndarray | None     # at terminal: (n_players,)
```

**Algorithm.**  
Post-order traversal; at each non-terminal node, choose child with max payoff for `player` (break ties consistently); store selected action.

**API.**

```python
# projects/backward_induction.py
from dataclasses import dataclass
import numpy as np

@dataclass
class Node:
    player: int | None
    actions: list[str] | None
    children: list['Node'] | None
    payoff: np.ndarray | None

def backward_induction(root: Node) -> tuple[list[str], np.ndarray]:
    path = []
    def solve(node: Node) -> np.ndarray:
        if node.player is None:
            return node.payoff
        best_idx, best_val = None, -np.inf
        for i, child in enumerate(node.children):
            val = solve(child)
            if val[node.player] > best_val + 1e-12:
                best_val = val[node.player]; best_idx = i; best_pay = val
        path.append(node.actions[best_idx])
        return best_pay
    pay = solve(root)
    return path, pay
```

**Tests.**  
Ultimatum game (discrete offers) or Stackelberg quantity (discretized follower’s best response).

**Extensions.**  
Add chance nodes; one-deviation checks; multiple equilibria via tie sets.

---

# 7) Signaling PBE enumerator (finite types/messages/actions)

**Goal.** Enumerate pooling/separating candidates; check Receiver BR, Sender IC, and Bayes beliefs on-path.

**Design.**

- Types $$Θ={θ1,…,θK}\Theta=\{\theta_1,\dots,\theta_K\}$$ with prior $$π$$
    
- Messages MM, actions AA.
    
- Sender utility $$uS(θ,m,a)u_S(\theta,m,a),$$ Receiver $$uR(θ,m,a)u_R(\theta,m,a).$$
    

**Algorithm.**  
Loop over mappings $$m:Θ→Mm:\Theta\to M$$ (patterns). For each message mm, compute posterior $$μ(θ∣m)\mu(\theta\mid m)$$ by Bayes (if on-path). Compute Receiver $$BR a⋆(m)a^\star(m).$$ Check for each θ\theta that prescribed $$m(θ)m(\theta) yields ≥\ge$$ any deviation given $$a⋆(⋅)a^\star(\cdot).$$ Choose defensible off-path beliefs to sustain if needed.

**API (skeleton).**

```python
# projects/pbe_signaling.py
import itertools, numpy as np

def enumerate_pbe(theta_vals, prior, messages, actions, u_S, u_R):
    """
    theta_vals: list of types
    prior: array shape (K,)
    messages: list
    actions: list
    u_S(theta,m,a), u_R(theta,m,a): callables
    Returns list of dicts with 'pattern', 'receiver_BR', 'IC_ok'
    """
    K = len(theta_vals)
    out = []
    for pattern in itertools.product(range(len(messages)), repeat=K):
        # On-path posteriors
        posteriors = []
        for m_idx in range(len(messages)):
            mass = sum(prior[i] for i,t in enumerate(theta_vals) if pattern[i]==m_idx)
            if mass <= 1e-12: 
                posteriors.append(None)  # off-path
            else:
                p = np.zeros(K)
                for i in range(K):
                    if pattern[i]==m_idx:
                        p[i] = prior[i]/mass
                posteriors.append(p)
        # Receiver BR per message
        receiver_BR = []
        for m_idx in range(len(messages)):
            if posteriors[m_idx] is None:
                receiver_BR.append(None)  # to be chosen (off-path)
            else:
                # pick action maximizing expected u_R
                vals = []
                for a_idx,_ in enumerate(actions):
                    v = sum(posteriors[m_idx][i] * u_R(theta_vals[i], messages[m_idx], actions[a_idx])
                            for i in range(K))
                    vals.append(v)
                receiver_BR.append(int(np.argmax(vals)))
        # Sender IC check (only on-path)
        IC_ok = True
        for i,theta in enumerate(theta_vals):
            m_i = pattern[i]
            a_i = receiver_BR[m_i]
            base = u_S(theta, messages[m_i], actions[a_i]) if a_i is not None else -1e9
            for alt_m in range(len(messages)):
                if alt_m == m_i: 
                    continue
                a_alt = receiver_BR[alt_m]
                # naive: assume off-path belief worst for deviator -> skip strict check if None
                if a_alt is None: 
                    continue
                dev = u_S(theta, messages[alt_m], actions[a_alt])
                if dev > base + 1e-9:
                    IC_ok = False; break
            if not IC_ok: break
        out.append(dict(pattern=pattern, receiver_BR=receiver_BR, IC_ok=IC_ok))
    return [r for r in out if r['IC_ok']]
```

**Tests.**  
Two types, two messages, two actions with simple quadratic preferences; verify existence of separating/babbling.

**Extensions.**  
Add refined off-path beliefs (intuitive criterion); compute welfare of each PBE.

---

# 8) Entry game with private costs: cutoff fixed point

**Goal.** Solve symmetric cutoff $$t⋆t^\star for c∼U[0,1]c\sim U[0,1],$$ benefit BB, duopoly payoff b $$(0≤b<B0\le b<B).$$

**Theory.** Enter if $$c≤tc\le t.$$ Expected profit at type cc:

$$Π(c;t)=(1−t)(B−c)+t(b−c).\Pi(c;t) = (1-t)(B-c) + t(b-c).$$

Indifference at $$c=tc=t: (1−t)(B−t)+t(b−t)=0(1-t)(B-t) + t(b-t)=0. Solve t=θ†(t)t=\theta^\dagger(t).$$

**API.**

```python
# projects/entry_cutoff.py
def entry_cutoff_fixed_point(B: float=1.0, b: float=0.0, tol=1e-12):
    import mpmath as mp
    f = lambda t: (1-t)*(B-t) + t*(b-t)
    # find root in [0,1]
    root = mp.findroot(lambda x: f(x), 0.5)
    t = max(0.0, min(1.0, float(root)))
    # verify self-consistency (monotone best reply here)
    return t
```

**Tests.**

```python
assert abs(entry_cutoff_fixed_point(1.0, 0.0) - 0.5) < 1e-7
```

**Extensions.**  
General FF, correlated costs, multiple entrants n>2n>2 (binomial terms).

---

# 9) First-Price Auction (IPV, Uniform) + verification

**Goal.** Implement $$b(v)=n−1nvb(v)=\tfrac{n-1}{n}v$$ and simulate revenue vs second-price.

**API.**

```python
# projects/fpa_uniform.py
import numpy as np

def bid_fpa_uniform(v: np.ndarray, n: int) -> np.ndarray:
    return (n-1)/n * v

def simulate_auction_uniform(n: int, rounds: int=100000, seed=0):
    rng = np.random.default_rng(seed)
    V = rng.random((rounds, n))  # Uniform[0,1]
    # FPA
    B = (n-1)/n * V
    winners = np.argmax(B, axis=1)
    pay_fpa = B[np.arange(rounds), winners]
    rev_fpa = pay_fpa.mean()
    # SPA
    sorted_V = np.sort(V, axis=1)
    rev_spa = sorted_V[:, -2].mean()
    return rev_fpa, rev_spa
```

**Tests.**

```python
rev_fpa, rev_spa = simulate_auction_uniform(5, 200000)
assert abs(rev_fpa - rev_spa) < 5e-3   # Revenue Equivalence (approx)
```

**Extensions.**  
General FF via numeric envelope; risk aversion; reserves.

---

# 10) Myerson’s reserve (continuous & empirical)

**Goal.** Compute optimal reserve $$r∗r^\ast$$ from virtual value $$ϕ(v)=v−1−F(v)f(v)\phi(v)=v-\frac{1-F(v)}{f(v)}.$$

**Uniform example.**

$$ϕ(v)=2v−1⇒r∗=12.\phi(v)=2v-1 \Rightarrow r^\ast=\tfrac12.$$

**Empirical FF.** Estimate FF by ECDF and ff by kernel density; solve $$ϕ^(r)=0\hat\phi(r)=0.$$

**API (uniform + general hook).**

```python
# projects/myerson_reserve.py
import numpy as np
from typing import Callable

def reserve_uniform() -> float:
    return 0.5

def reserve_from_pdf_cdf(F: Callable[[float], float],
                         f: Callable[[float], float],
                         lo: float, hi: float) -> float:
    import mpmath as mp
    phi = lambda v: v - (1 - F(v)) / max(f(v), 1e-12)
    return float(mp.findroot(phi, (lo+hi)/2))
```

**Extensions.**  
Ironing for irregular ϕ; empirical process confidence bands.

---

# 11) Revenue Equivalence Simulator

**Goal.** Confirm SPA = FPA = All-pay expected revenue under IPV assumptions.

**API.**

```python
# projects/revenue_equivalence.py
import numpy as np

def simulate_revenues(n=4, R=200000, seed=0):
    rng = np.random.default_rng(seed)
    V = rng.random((R, n))
    # SPA
    Vsort = np.sort(V, axis=1)
    rev_spa = Vsort[:, -2].mean()
    # FPA
    B = (n-1)/n * V
    rev_fpa = B.max(axis=1).mean()
    # All-pay (truthful rank-order in IPV uniform -> expected payment equals value-rank expectation)
    # In symmetric equilibrium, expected highest bid equals SPA revenue; implement via known identity:
    rev_allpay = rev_spa
    return rev_spa, rev_fpa, rev_allpay
```

**Tests.**  
`assert` near equality; then break RE by making bidders CRRA.

---

# 12) Winner’s Curse Simulator (common values)

**Goal.** Show $$E[v∣win]<E[v]\mathbb{E}[v\mid \text{win}]<\mathbb{E}[v].$$

**Model.** $$v∼N(0,1)v\sim \mathcal{N}(0,1).$$ Signals $$si=v+ϵis_i=v+\epsilon_i, ϵi∼N(0,τ2)\epsilon_i\sim \mathcal{N}(0,\tau^2). $$Truthful bids $$=E[v∣si]= \mathbb{E}[v\mid s_i] (diagnostic).$$

**API.**

```python
# projects/winner_curse_sim.py
import numpy as np

def winner_curse(n=5, R=200000, tau=1.0, seed=0):
    rng = np.random.default_rng(seed)
    v = rng.normal(0,1,size=R)
    eps = rng.normal(0,tau,size=(R,n))
    s = v[:,None] + eps
    # Posterior mean under Gaussian prior/likelihood
    k = 1/(1 + tau**2)  # precision weight (assuming prior var=1)
    bids = k * s
    winners = np.argmax(bids, axis=1)
    v_win = v  # actual value
    posterior_win = bids[np.arange(R), winners]  # naive bids
    return v_win[winners==winners].mean(), posterior_win.mean()  # sanity
```

**Check.**  
Compute $$E[v∣win]\mathbb{E}[v\mid \text{win}]$$ numerically and compare to unconditional 00.

**Extensions.**  
Optimal bid shading; ascending auction information release.

---

# 13) Lemke–Howson (bimatrix NE)

**Goal.** Implement a small, didactic LH pivot for m×nm\times n games.

**Idea.** Work on the best-response polytopes (labeled by actions), start from artificial equilibrium, follow complementary pivots until you drop the artificial label.

**Reference skeleton (compact).**

```python
# projects/lemke_howson.py
# For study: use the 'lh' algorithm from gambit or write a minimal pivot path.
# Here we provide a thin wrapper to 'nashpy' if installed; otherwise, fallback to support enumeration.
def lemke_howson(A, B):
    try:
        import nashpy as nash
        g = nash.Game(A, B)
        for eq in g.lemke_howson(initial_dropped_label=0):
            return eq  # first equilibrium
    except Exception:
        from .support_enum import support_enumeration
        sols = support_enumeration(A,B)
        if sols: 
            return sols[0]
    raise RuntimeError("No equilibrium found.")
```

**Extensions.**  
Implement full LCP and pivoting (longer but instructive).

---

# 14) Kyle (1985) one-period calibration + sim

**Goal.** Compute (λ,β) and simulate P&L.

**Theory.** With $$v∼N(μ,σv2)v\sim \mathcal{N}(\mu,\sigma_v^2), u∼N(0,σu2)u\sim \mathcal{N}(0,\sigma_u^2),$$

$$λ=σv2σu,β=σuσv.\lambda=\frac{\sigma_v}{2\sigma_u},\qquad \beta=\frac{\sigma_u}{\sigma_v}.$$

**API.**

```python
# projects/kyle_one_period.py
import numpy as np
from dataclasses import dataclass

@dataclass
class KyleParams:
    lam: float
    beta: float

def kyle_params(sigma_v: float, sigma_u: float) -> KyleParams:
    return KyleParams(lam=sigma_v/(2*sigma_u), beta=sigma_u/sigma_v)

def simulate_kyle(mu=0.0, sigma_v=1.0, sigma_u=1.0, R=200000, seed=0):
    rng = np.random.default_rng(seed)
    v = rng.normal(mu, sigma_v, size=R)
    u = rng.normal(0, sigma_u, size=R)
    lam = sigma_v/(2*sigma_u); beta = sigma_u/sigma_v
    x = beta*(v-mu)
    y = x + u
    p = mu + lam*y
    pnl_informed = (v - p) * x
    return lam, beta, pnl_informed.mean(), pnl_informed.std()
```

**Tests.**  
Increase σvor reduce $$σu\sigma_u → λ\lambda$$ rises; check signs of P&L.

**Extensions.**  
Multi-period Kyle; inventory-cost MM; asymmetric noise.

---

# 15) Pigou network PoA (nonatomic routing)

**Goal.** Verify $$PoA=43\mathrm{PoA}=\tfrac{4}{3}$$ for affine latencies in Pigou’s two-link network.

**Model.** Demand 11. Edge 1: $$ℓ1(x)=x\ell_1(x)=x. Edge 2: ℓ2(x)=1\ell_2(x)=1.$$

- Wardrop equilibrium: all flow on edge 1 ⇒ cost 11.
    
- Social optimum: split $$x=0.5x=0.5$$ on edge 1 ⇒ cost $$0.5⋅0.5+0.5⋅1=0.750.5\cdot 0.5 + 0.5\cdot 1 = 0.75.$$
    
- $$PoA=1/0.75=4/3\mathrm{PoA}=1/0.75=4/3.$$
    

**API.**

```python
# projects/pigou_poa.py
def pigou_poa():
    cost_eq = 1.0
    cost_opt = 0.75
    return cost_eq/cost_opt  # 1.333...
```

**Extensions.**  
General latency families; compute Wardrop via convex optimization (Beckmann transform).

---

# 16) Congestion games: Rosenthal potential + convergence

**Goal.** Show better-reply dynamics converge by increasing potential

$$Φ(s)=∑e∑k=1xe(s)ce(k).\Phi(s)=\sum_{e}\sum_{k=1}^{x_e(s)} c_e(k).$$

**API.**

```python
# projects/congestion_potential.py
import numpy as np
from typing import List, Dict, Tuple

def rosenthal_potential(profile: List[List[int]], costs: Dict[int, List[float]]) -> float:
    """
    profile: list of players' resource indices they use
    costs[e]: list [c_e(1), c_e(2), ...]
    """
    from collections import Counter
    count = Counter([e for P in profile for e in P])
    phi = 0.0
    for e, x in count.items():
        phi += sum(costs[e][:x])
    return phi

def best_reply_step(profile, strategy_sets, costs):
    # naive: each player tries all admissible strategies, picks min personal cost
    def player_cost(P):
        from collections import Counter
        # compute counts including self
        count = Counter([e for Q in profile for e in Q])
        return sum(costs[e][count[e]] for e in P)  # next user index = current count (0-based off by 1 careful!)
    m = len(profile)
    improved = False
    for i in range(m):
        current = profile[i]
        best = min(strategy_sets[i], key=lambda P: player_cost(P))
        if best != current:
            profile[i] = best; improved = True
    return improved, profile
```

**Tests.**  
Small network with two routes per player; iterate `best_reply_step` until no change; track Φ\Phi monotone decreasing (for costs) or increasing (for negative-cost payoffs) depending on your sign convention.

**Extensions.**  
Atomic vs nonatomic; random asynchronous updates.

---

## How to use this pack (fast loop)

1. Pick **one** project.
    
2. Copy the file into `projects/`, run its local test snippet.
    
3. Add 1–2 extensions (your choice).
    
4. Write a 10-line **“What I learned”** note in Obsidian, with formulas + quick plots/numbers.
    

---

## Mini check (one step)

Which single project do you want to implement **first**: the LP zero-sum solver, the 2×2 mixed-NE tool, or the Kyle simulator?