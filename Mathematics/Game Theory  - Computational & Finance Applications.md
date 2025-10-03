#math #game_theory #quantitative_finance 

Amazing—Week 6 is where we glue theory to computation and quant practice. We’ll touch **computational game theory**, **evolutionary/continuous-time dynamics**, and two **finance workhorses**: the one-period **Kyle model** and the flavor of **mean-field games** behind crowding/execution effects. I’ll keep all math in LaTeX\LaTeX for Obsidian.

---

# Week 6 — Computation, Dynamics, and Quant Applications

## 0) Learning goals

By the end you should be able to:

1. Recognize when a game is **easy vs hard** computationally (LP for zero-sum; PPAD for general bimatrix).
    
2. Solve small bimatrix games with **support enumeration / Lemke–Howson**, and read the **LCP** conditions.
    
3. Use **replicator dynamics** and know how it connects to **ESS**, **NE**, and **MWU**.
    
4. Write the basic **HJI/HJB** equations for differential games and the **HJB–Fokker–Planck** system for mean-field games.
    
5. Derive and interpret the **Kyle (1985)** linear equilibrium $$(x=β(v−μ), p=μ+λy)(x=\beta(v-\mu),\ p=\mu+\lambda y)$$ and what $$λ,β\lambda,\beta$$ scale with.
    

---

## 1) Computational game theory: what’s tractable?

### 1.1 Zero-sum (matrix) games = LP (polytime)

A zero-sum game with payoff matrix AA is equivalent to the LP pair:

$$max⁡p,vvs.t.p⊤A≥v 1⊤,1⊤p=1, p≥0,min⁡q,wws.t.Aq≤w 1,1⊤q=1, q≥0.$$$$\begin{aligned} \max_{p,v}\quad & v \\ \text{s.t.}\quad & p^\top A \ge v\,\mathbf{1}^\top,\quad \mathbf{1}^\top p=1,\ p\ge 0, \end{aligned} \qquad \begin{aligned} \min_{q,w}\quad & w \\ \text{s.t.}\quad & A q \le w\,\mathbf{1},\quad \mathbf{1}^\top q=1,\ q\ge 0. \end{aligned}$$

Strong duality gives the value and optimal mixes in polynomial time.

### 1.2 General two-player games (bimatrix) = PPAD

Finding an exact Nash equilibrium in a general bimatrix game is **PPAD-complete** (believed not polytime in general). Practical solvers exist for small/medium cases.

**Key algorithms**

- **Support enumeration:** guess supports $$S1,S2S_1,S_2,$$ solve the **indifference conditions** (equalize payoffs on support, off-support ≤\le).
    
- **Lemke–Howson (LH):** a complementary-pivot path on labeled best-response polytopes; finite termination, exponential worst-case but good in practice for small games.
    

### 1.3 Bimatrix NE as an LCP

A bimatrix game $$(A,B)(A,B)$$ can be written as an LCP find$$ z≥0 s.t. Mz+q≥0, z⊤(Mz+q)=0 \text{find } z\ge 0 \text{ s.t. } Mz+q\ge 0,\ z^\top(Mz+q)=0.$$ One common form uses slack variables for best-response inequalities and normalizations $$x⊤1=1, y⊤1=1x^\top \mathbf{1}=1,\ y^\top \mathbf{1}=1.$$ The **complementarity** encodes “play action kk only if its slack is zero (tied at the top).”

---

## 2) Evolutionary / learning dynamics

### 2.1 Replicator dynamics (symmetric population games)

Let $$A∈Rm×mA\in\mathbb{R}^{m\times m}$$ be the payoff matrix of a symmetric game; population state $$x∈Δmx\in\Delta^{m}.$$ Payoff to pure strategy ii is $$πi(x)=(Ax)i\pi_i(x)=(Ax)_i;$$ population average $$πˉ(x)=x⊤Ax\bar\pi(x)=x^\top A x.$$

**Continuous-time replicator ODE**

$$x˙i  =  xi(πi(x)−πˉ(x)),i=1,…,m.\dot x_i \;=\; x_i\big(\pi_i(x)-\bar\pi(x)\big),\qquad i=1,\dots,m.$$

- **Rest points** satisfy: if $$xi>0x_i>0$$ then $$πi(x)=πˉ(x)\pi_i(x)=\bar\pi(x);$$ if $$xi=0x_i=0$$ then $$πi(x)≤πˉ(x)\pi_i(x)\le \bar\pi(x).$$ In symmetric games these are (generically) **Nash equilibria**.
    
- **ESS (evolutionarily stable strategy):** a symmetric $$NE x∗x^\ast$$ is **ESS** iff for all $$y≠x∗y\neq x^\ast$$ sufficiently close,
    

$$x∗⊤Ax∗  ≥  y⊤Ax∗,$$and if equality then $$x∗⊤Ay  >  y⊤Ay.x^{\ast\top} A x^\ast \;\ge\; y^\top A x^\ast,\quad \text{and if equality then } x^{\ast\top} A y \;>\; y^\top A y.$$

ESS are (locally) **asymptotically stable** under replicator in many classes.

**Rock–Paper–Scissors (zero-sum)**: interior mixed NE is **neutrally stable**; trajectories cycle around it; time averages converge to the NE.

### 2.2 Discrete-time replicator and MWU

A normalized discrete replicator step,

$$xt+1,i∝xt,i exp⁡(η πi(xt)),x_{t+1,i} \propto x_{t,i}\,\exp\big(\eta\,\pi_i(x_t)\big),$$

is the **multiplicative weights update**. No-regret MWU ⇒ empirical play approaches **coarse correlated equilibria**; in zero-sum, time-averaged play converges to the value mixed strategy.

---

## 3) Differential games (brief, but powerful)

Consider a two-player zero-sum differential game:

$$x˙t  =  f(xt,ut,vt),J(u,v)=∫0Tℓ(xt,ut,vt) dt+g(xT).\dot{x}_t \;=\; f(x_t,u_t,v_t),\qquad J(u,v)=\int_0^T \ell(x_t,u_t,v_t)\,dt + g(x_T).$$

Define the **Isaacs Hamiltonian**

$$H(x,p)  =  min⁡umax⁡v {ℓ(x,u,v)+p⊤f(x,u,v)}.H(x,p)\;=\;\min_{u}\max_{v}\ \big\{\ell(x,u,v)+p^\top f(x,u,v)\big\}.$$

If the **Isaacs condition** (min–max = max–min) holds, the value V(t,x)V(t,x) solves the **Hamilton–Jacobi–Isaacs (HJI)** equation:

$$−∂tV(t,x)  =  H(x,∇xV(t,x)),V(T,x)=g(x).-\partial_t V(t,x)\;=\; H\big(x,\nabla_x V(t,x)\big),\qquad V(T,x)=g(x).$$

In general-sum settings, each player solves an HJB with coupling through dynamics or costs; feedback Nash equilibria satisfy coupled HJBs.

---

## 4) Mean-field games (MFG): crowding & systemic effects

A continuum of agents (mass 11) with state distribution $$mt(x)m_t(x).$$ Typical **deterministic** MFG system (Lasry–Lions) with control ata_t and drift $$b(x,a,mt)b(x,a,m_t):$$

**Backward HJB (value function uu)**

$$−∂tu(t,x)  =  H ⁣(x,∇u(t,x),mt)+f(x,mt),u(T,x)=g(x,mT).-\partial_t u(t,x) \;=\; H\!\left(x,\nabla u(t,x),m_t\right) + f(x,m_t),\qquad u(T,x)=g(x,m_T).$$

**Forward continuity (Fokker–Planck)**

$$∂tmt(x)  +  ∇ ⁣⋅ ⁣(mt(x) ∇pH ⁣(x,∇u(t,x),mt))  =  0,m0 given.\partial_t m_t(x) \;+\; \nabla\!\cdot\!\Big(m_t(x)\, \nabla_p H\!\left(x,\nabla u(t,x),m_t\right)\Big) \;=\; 0,\qquad m_0 \text{ given}.$$

Stochastic variants add diffusion terms $$νΔu\nu\Delta u and νΔm\nu\Delta m.$$

**Linear–quadratic (LQ) MFG:** Quadratic costs + linear dynamics yield Riccati equations and mean-field fixed points; widely used for **optimal execution with crowding**: each trader penalizes deviations from a mean inventory/flow and faces price impact driven by aggregate flow mtm_t.

---

## 5) Market microstructure: the one-period Kyle model

**Setup.** Fundamental value $$v∼N(μ,σv2)v\sim \mathcal{N}(\mu,\sigma_v^2).$$  
Noise order $$u∼N(0,σu2)u\sim \mathcal{N}(0,\sigma_u^2),$$ independent of vv.  
An informed trader observes vv and submits order xx. Total order flow $$y=x+uy=x+u.$$  
Competitive market maker sets price $$p=E[v∣y]p=\mathbb{E}[v\mid y].$$

**Linear equilibrium ansatz**

$$x=β (v−μ),p=μ+λ y.x=\beta\,(v-\mu),\qquad p=\mu+\lambda\,y.$$

- **MM efficiency:** for Gaussian primitives,
    

$$λ  =  Cov⁡(v,y)Var⁡(y)  =  β σv2β2σv2+σu2.\lambda \;=\; \frac{\operatorname{Cov}(v,y)}{\operatorname{Var}(y)} \;=\; \frac{\beta\,\sigma_v^2}{\beta^2\sigma_v^2+\sigma_u^2}.$$

- **Insider’s problem (given λ\lambda):** maximize $$E[(v−p) x ∣ v]=(v−μ) x−λx2\mathbb{E}\big[(v-p)\,x\,\big|\,v\big]= (v-\mu)\,x-\lambda x^2 ⇒$$ first-order condition gives
    

$$x∗(v)  =  v−μ2λ⇒β  =  12λ.x^\ast(v) \;=\; \frac{v-\mu}{2\lambda} \quad\Rightarrow\quad \beta \;=\; \frac{1}{2\lambda}.$$

- **Solve for (λ,β):** substitute $$β=1/(2λ)\beta=1/(2\lambda)$$ into the MM formula to get
    

 $$λ  =  σv2 σu , β  =  σuσv .\boxed{\ \lambda \;=\; \frac{\sigma_v}{2\,\sigma_u}\ },\qquad \boxed{\ \beta \;=\; \frac{\sigma_u}{\sigma_v}\ }.$$

**Interpretation.**

- **Price impact** λ\lambda is larger when **fundamental uncertainty** $$σv\sigma_v$$ is high or **noise liquidity** σu\sigma_u is low.
    
- Insider trades **more aggressively** (β) when noise liquidity is abundant (u) or the signal is precise (σ).
    
- The MM’s price is the **Bayes posterior** $$p=E[v∣y]p=\mathbb{E}[v\mid y].$$ Variance decompositions give you residual information in price after one auction.
    

_Notes._ In multi-period Kyle, λ typically decreases over time (information gradually impounded). Risk aversion/inventory costs modify the linear coefficients.

---

## 6) Tying it together (why this matters in quant)

- **Algorithmic feasibility:** zero-sum LPs are safe to automate; general bimatrix may need heuristics (LH/support enumeration) or approximations.
    
- **Dynamics as learning:** replicator ↔ MWU explains why simple adaptive policies stabilize near NE/CCE in adversarial markets.
    
- **Control + equilibrium:** HJB/HJI/MFG is the language for **execution**, **market making**, and **crowding** (your policy must be optimal _and_ consistent with everyone else’s).
    

---

## 7) Worked mini-templates

### 7.1 Indifference conditions (bimatrix)

For supports $$S1,S2S_1,S_2,$$ let xx (row) and yy (column) be mixed strategies. On support,

$$(Ay)i  =  α  for i∈S1,(Ay)i≤α  for i∉S1,(Ay)_i \;=\; \alpha \ \text{ for } i\in S_1,\quad (Ay)_i \le \alpha \ \text{ for } i\notin S_1, (B⊤x)j  =  β  for j∈S2,(B⊤x)j≤β  for j∉S2,(B^\top x)_j \;=\; \beta \ \text{ for } j\in S_2,\quad (B^\top x)_j \le \beta \ \text{ for } j\notin S_2,$$

with $$x≥0, 1⊤x=1, y≥0, 1⊤y=1x\ge 0,\ \mathbf{1}^\top x=1,\ y\ge 0,\ \mathbf{1}^\top y=1.$$

### 7.2 Replicator and ESS check

Pick $$x∗x^\ast.$$ Verify (i) symmetric NE conditions on support; (ii) **ESS** inequality above. If ESS, expect local asymptotic stability under replicator.

### 7.3 Kyle quick-solve

Use the two equations

$$λ=βσv2β2σv2+σu2,β=12λ\lambda=\frac{\beta\sigma_v^2}{\beta^2\sigma_v^2+\sigma_u^2},\qquad \beta=\frac{1}{2\lambda}$$

to recover λ,β in two lines (as shown).

---

## 8) Optional coding (1–2 h total)

- **Replicator simulator:** for $$A=[0−110]A=\begin{bmatrix}0&-1\\ 1&0\end{bmatrix}$$$ (matching pennies payoff to row), integrate $$x˙=x⊙(Ax−x⊤Ax 1)\dot x = x\odot(Ax - x^\top A x\,\mathbf{1});$$ plot trajectories on $$Δ2\Delta^2.$$$
    
- **Lemke–Howson demo:** implement a small pivot routine for 3×33\times 3 games; compare to support enumeration.
    
- **Kyle calibration:** given $$(σ^v,σ^u)(\hat\sigma_v,\hat\sigma_u)$$ from data, compute (λ,β); simulate order flow and posterior pricing.
    

---

## 9) Common pitfalls

- Expecting **polynomial** algorithms for general bimatrix NE (use LH/heuristics; keep sizes small).
    
- Interpreting **replicator fixed points** as globally stable (zero-sum cycles!).
    
- Writing HJB/HJI without checking **Isaacs condition** or boundary/terminal conditions.
    
- Forgetting that λ\lambda in Kyle emerges from **Bayes**; it is **not** arbitrary “impact.”
    

---

### Quick check (one step)

In the one-period Kyle model above, fix $$λ>0\lambda>0.$$ For a realized v, the informed trader solves

$$max⁡x  (v−μ) x−λx2.\max_{x}\ \ (v-\mu)\,x-\lambda x^2.$$

What $$x∗(v)x^\ast(v)$$ maximizes this quadratic? (Just give the expression in v,μ,λv; we’ll use it to recover β next.)