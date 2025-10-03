#math #game_theory #quantitative_finance

Perfect—here’s your **Week 0 deep dive**. Treat this as a mini-chapter you can read in ~2–3 hours and refer back to during the course. It’s deliberately game-theory-centric: every item is here because it’s used over and over in existence/uniqueness proofs, equilibrium computation, and mechanism design.

---

# 0. Learning goals

By the end you should be able to:

1. recognize when a best-response map satisfies a fixed-point theorem (→ Nash existence);
    
2. solve tiny zero-sum games by **LP duality** and read KKT conditions;
    
3. apply the **envelope theorem** to get payments/information rents;
    
4. do clean **Bayesian updates** and use the **law(s) of iterated expectations**.
    

---

# 1) Convex analysis & fixed points (Brouwer / Kakutani)

## 1.1 Convexity, compactness, continuity (the three C’s)

- A set $$C⊂RnC\subset \mathbb{R}^n$$ is **convex** if $$x,y∈C⇒λx+(1−λ)y∈Cx,y\in C\Rightarrow \lambda x+(1-\lambda)y\in C for all λ∈[0,1]\lambda\in[0,1].$$
    
- A function ff is **convex** if $$f(λx+(1−λ)y)≤λf(x)+(1−λ)f(y)f(\lambda x+(1-\lambda)y)\le \lambda f(x)+(1-\lambda)f(y).$$
    
- **Compact** = closed and bounded (in $$Rn\mathbb{R}^n$$); compact + continuous ⇒ maxima/minima exist (Weierstrass).
    

Why you care in games:

- Mixed strategies live on **simplices** $$\Delta(S)$$: convex + compact.
    
- Expected payoffs are **linear** in each player’s mixed strategy ⇒ best-response sets are convex (key for Kakutani).
    

## 1.2 Correspondences & upper hemicontinuity (UHC)

A **correspondence** $$F:X⇉YF:X\rightrightarrows Y maps each x∈Xx\in X to a **set** F(x)⊆YF(x)\subseteq Y.$$

- FF is **upper hemicontinuous** at xx if for every open $$V⊇F(x)V\supseteq F(x)$$ there exists a neighborhood UU of xx with $$F(x′)⊆VF(x')\subseteq V for all x′∈Ux'\in U.$$ 
    Equivalent practical test (on compact domains): **closed graph** + **compact values** ⇒ UHC.
    

Why you care:

- The **best-response correspondence** $$BRi(σ−i)BR_i(\sigma_{-i})$$ is non-empty, convex-valued, and UHC under standard assumptions (finite actions, mixed strategies, continuous expected payoffs). This puts you in Kakutani territory.
    

## 1.3 Fixed-point theorems (statements you’ll use)

- **Brouwer** (single-valued): A continuous function $$f:K→Kf:K\to K$$ on a non-empty compact convex $$K⊂RnK\subset\mathbb{R}^n$$ has a fixed point $$x∗=f(x∗)x^\ast=f(x^\ast).$$
    
- **Kakutani** (set-valued): If $$F:K⇉KF:K\rightrightarrows K$$ has non-empty, compact, **convex** values and is **UHC**, then there exists $$x∗∈Kx^\ast\in K with x∗∈F(x∗)x^\ast\in F(x^\ast).$$
    

How this proves Nash existence (high-level):

- Let $$K=∏iΔ(Si)K=\prod_i \Delta(S_i)$$ (product of simplices; convex & compact).
    
- Define $$F(σ)=∏iBRi(σ−i)F(\sigma)=\prod_i BR_i(\sigma_{-i}). $$ 
    If each $$BRiBR_i is non-empty, convex-valued, UHC, Kakutani ⇒ a fixed point σ∗∈F(σ∗)\sigma^\ast\in F(\sigma^\ast), i.e., σi∗∈BRi(σ−i∗)\sigma^\ast_i\in BR_i(\sigma^\ast_{-i}) for all ii ⇒ **Nash equilibrium**.$$
    

### Quick checks you’ll re-use

- **Non-emptiness:** argmax over compact simplex of a continuous (linear) expected payoff.
    
- **Convex-valued:** argmax of a **linear** function over a convex set is convex (ties mix).
    
- **UHC:** Berge’s Maximum Theorem: if payoffs are continuous and the feasible set is continuous/compact, the argmax correspondence is non-empty, compact-valued, UHC.
    

---

# 2) Linear programming (LP), duality & KKT

## 2.1 LP in primal/dual standard forms

Primal (max form):

maximize $$c⊤xs.t. Ax≤b,  x≥0.\text{maximize } c^\top x \quad \text{s.t. } Ax \le b,\; x\ge 0.$$

Dual (min form):

minimize $$b⊤ys.t. A⊤y≥c,  y≥0.\text{minimize } b^\top y \quad \text{s.t. } A^\top y \ge c,\; y\ge 0.$$

- **Weak duality:** $$c⊤x≤b⊤yc^\top x \le b^\top$$ y for any feasible x,yx,y.
    
- **Strong duality:** at optimum, equality holds (under mild regularity, always for LPs).
    
- **Complementary slackness:** $$yj(bj−aj⊤x)=0y_j(b_j - a_j^\top x)=0, xi((A⊤y−c)i)=0x_i((A^\top y - c)_i)=0.$$
    

## 2.2 Zero-sum matrix games = LP duals (minimax)

Let player 1 choose a mixed strategy $$p∈Δmp\in\Delta_m$$ over rows; player 2 chooses $$q∈Δnq\in\Delta_n$$ over columns; payoff matrix AA

Player 1’s value vv solves:

$$max⁡p∈Δmmin⁡j  p⊤Aej⟺max⁡p,v  vs.t. p⊤A≥v1⊤,  p≥0,  1⊤p=1.\max_{p\in \Delta_m} \min_{j} \; p^\top A e_j \quad\Longleftrightarrow\quad \begin{aligned} &\max_{p,v} \; v \\ &\text{s.t. } p^\top A \ge v \mathbf{1}^\top,\; p\ge 0,\; \mathbf{1}^\top p=1. \end{aligned}$$

Dual (player 2):

$$min⁡q,w  ws.t. Aq≤w1,  q≥0,  1⊤q=1.\min_{q,w} \; w \quad \text{s.t. } A q \le w \mathbf{1},\; q\ge 0,\; \mathbf{1}^\top q=1.$$

At optimum $$v∗=w∗v^\ast=w^\ast$$ (**von Neumann minimax**).  
Intuition: the **dual variables are the opponent’s mixed strategy**.

### Worked micro-example — Matching Pennies

$$A=(1−1−11)A=\begin{pmatrix}1 & -1\\ -1 & 1\end{pmatrix}.$$  
$$Symmetry ⇒ p=(12,12)p=(\tfrac12,\tfrac12), q=(12,12)q=(\tfrac12,\tfrac12), value v∗=0v^\ast=0.  $$
You can derive it by plugging into either LP: constraints bind equally, complementary slackness forces equalization of payoffs on support.

## 2.3 KKT conditions (nonlinear generalization)

For a smooth convex program

$$min⁡xf(x)  s.t.  gi(x)≤0,  hj(x)=0,\min_x f(x)\;\text{s.t.}\; g_i(x)\le 0,\; h_j(x)=0,$$

the **KKT conditions** (necessary; and sufficient under convexity + regularity) are:

- **Primal feasibility**: $$gi(x∗)≤0,  hj(x∗)=0g_i(x^\ast)\le 0,\; h_j(x^\ast)=0.$$
    
- **Dual feasibility**: $$λi∗≥0\lambda_i^\ast\ge 0.$$
    
- **Stationarity**: $$∇f(x∗)+∑iλi∗∇gi(x∗)+∑jμj∗∇hj(x∗)=0\nabla f(x^\ast)+\sum_i \lambda_i^\ast \nabla g_i(x^\ast) + \sum_j \mu_j^\ast \nabla h_j(x^\ast)=0.$$
    
- **Complementary slackness**: $$λi∗gi(x∗)=0\lambda_i^\ast g_i(x^\ast)=0.$$
    

Game-theory lens:

- Dual variables = **shadow prices** / incentives.
    
- In mechanism design, KKT behind the scenes enforces **monotonicity** and IC/IR via Lagrange multipliers.
    

---

# 3) Envelope theorems (value derivatives with respect to parameters)

Let $$V(θ)=max⁡x∈Xf(x,θ)V(\theta)=\max_{x\in X} f(x,\theta)$$ with **unique** optimizer $$x∗(θ)x^\ast(\theta)$$ and regularity (continuous differentiability and standard constraint quals). Then:

- **Unconstrained envelope**:
    

$$dVdθ=∂f∂θ(x∗(θ),θ).\frac{dV}{d\theta} = \frac{\partial f}{\partial \theta}(x^\ast(\theta),\theta).$$

- **Constrained envelope** via Lagrangian: $$L=f+λ⊤g+μ⊤hL=f+\lambda^\top g+\mu^\top h):
    

dVdθ=∂L∂θ(x∗,λ∗,μ∗,θ).\frac{dV}{d\theta} = \frac{\partial L}{\partial \theta}\big(x^\ast,\lambda^\ast,\mu^\ast,\theta\big).$$

Mechanism-design one-liner (single-parameter, IC):

- If an agent with type $$θ\theta$$ gets allocation $q(θ)q(\theta)$$ and pays $$t(θ)t(\theta)$$, utility $$u(θ)=θq(θ)−t(θ)u(\theta)=\theta q(\theta)-t(\theta)$$ (quasi-linear).
    
- Under truth-telling and regularity, **Envelope** ⇒
    

$$u′(θ)=q(θ)⟹u(θ)=u(θ‾)+∫θ‾θq(s) ds,u'(\theta)=q(\theta)\quad\Longrightarrow\quad u(\theta)=u(\underline\theta)+\int_{\underline\theta}^{\theta} q(s)\,ds,$$

so payments follow

$$t(θ)=θq(θ)−u(θ)=θq(θ)−u(θ‾)−∫θ‾θq(s) ds.t(\theta)=\theta q(\theta)-u(\theta) = \theta q(\theta)-u(\underline\theta)-\int_{\underline\theta}^{\theta} q(s)\,ds.$$

Set $$u(θ‾)=0u(\underline\theta)=0$$ for IR at the lowest type to pin down the constant.  
This is the backbone of **auction payment formulas** and Myerson’s lemma.

---

# 4) Probability refresh for games: Bayes, expectations, beliefs

## 4.1 Bayes’ rule (densities and discrete)

Discrete:

$$Pr⁡(θ∣s)=Pr⁡(s∣θ)Pr⁡(θ)∑θ′Pr⁡(s∣θ′)Pr⁡(θ′).\Pr(\theta\mid s) = \frac{\Pr(s\mid \theta)\Pr(\theta)}{\sum_{\theta'} \Pr(s\mid \theta')\Pr(\theta')}.$$

Densities:

$$f(θ∣s)=f(s∣θ)f(θ)∫f(s∣t)f(t)dt.f(\theta\mid s) = \frac{f(s\mid \theta) f(\theta)}{\int f(s\mid t) f(t) dt}.$$

Useful forms:

- **Odds form:** $$Pr⁡(θ1∣s)Pr⁡(θ2∣s)=Pr⁡(θ1)Pr⁡(θ2)×f(s∣θ1)f(s∣θ2)\dfrac{\Pr(\theta_1\mid s)}{\Pr(\theta_2\mid s)} = \dfrac{\Pr(\theta_1)}{\Pr(\theta_2)} \times \dfrac{f(s\mid \theta_1)}{f(s\mid \theta_2)}.$$
    
- **Log-likelihood ratio** updates additively.
    

## 4.2 Conditional expectation & iterated expectations

- $$E[X∣F]\mathbb{E}[X\mid \mathcal{F}]$$ is the best $$\mathcal{F}$$-measurable predictor (orthogonal projection perspective).
    
- **Law of iterated expectations (tower):** $$E[ E[X∣Y] ]=E[X]\mathbb{E}[\,\mathbb{E}[X\mid Y]\,]=\mathbb{E}[X].$$
    
- **Bayes plausibility (info design):** Posterior beliefs average to the prior:  
    $$E[μ(θ∣s)]=μ0(θ)\mathbb{E}[\mu(\theta\mid s)]=\mu_0(\theta).$$
    

Why you care:

- **Bayesian Nash equilibrium**: strategies + beliefs must be mutually consistent under Bayes on-path.
    
- Microstructure/Kyle: market maker sets price = $$E[v∣order flow]\mathbb{E}[v\mid \text{order flow}].$$
    

### Micro example (clean numbers)

Two types $$θ∈{H,L}\theta\in\{H,L\}, prior Pr⁡(H)=0.4\Pr(H)=0.4. Signal s∈{+,−}s\in\{+, -\} with accuracy Pr⁡(s=+∣H)=0.7\Pr(s=+ \mid H)=0.7, Pr⁡(s=+∣L)=0.3\Pr(s=+ \mid L)=0.3. $$ 
Posterior:

$$Pr⁡(H∣+)=0.7⋅0.40.7⋅0.4+0.3⋅0.6=0.280.28+0.18=0.280.46≈0.6087.\Pr(H\mid +)=\frac{0.7\cdot 0.4}{0.7\cdot 0.4 + 0.3\cdot 0.6}=\frac{0.28}{0.28+0.18}=\frac{0.28}{0.46}\approx 0.6087. Pr⁡(H∣−)=0.3⋅0.40.3⋅0.4+0.7⋅0.6=0.120.12+0.42=0.120.54≈0.2222.\Pr(H\mid -)=\frac{0.3\cdot 0.4}{0.3\cdot 0.4 + 0.7\cdot 0.6}=\frac{0.12}{0.12+0.42}=\frac{0.12}{0.54}\approx 0.2222.$$

Iterated expectations check:$$ 0.6087Pr⁡(+)+0.2222Pr⁡(−)=0.40.6087\Pr(+)+0.2222\Pr(-)=0.4 (good).$$

---

# 5) How this plugs into game theory quickly

- **Nash existence:** You’ll repeatedly check: strategy simplices (compact convex); linear payoffs ⇒ convex best-responses; Berge ⇒ UHC; Kakutani ⇒ fixed point.
    
- **Zero-sum and computation:** LP duality = minimax; the opponent’s mixed strategy shows up as dual variables. Complementary slackness interprets **equalizing payoffs on the support**.
    
- **Mechanism design:** Envelope gives payments once you know monotone allocations; KKT/duality justify constraints like IC/IR cleanly.
    
- **Bayesian games:** Beliefs updated via Bayes; towers keep expectations consistent; “Bayes plausibility” is the sanity check in info design/signaling.
    

---

# 6) Typical pitfalls (avoid these)

- **Confusing convexity of functions with convexity of argmax sets.** You need linearity/concavity in the **player’s own** mix to ensure convex best-responses.
    
- **UHC vs continuity.** Best-response is a **correspondence**, not a function. Use Berge/closed-graph tests, don’t try to show “continuity.”
    
- **LP sign/shape mistakes.** Put games into a single payoff scale (shift the matrix if needed to make constraints $$≥v\ge v$$).
    
- **Envelope misuse.** You can’t differentiate through the argmax unless regularity holds (unique optimum or suitable constraint qualification). Beware kinks.
    

---

# 7) Fast exercises (with terse solutions right after)

### A. Convex best-responses

**Claim.** In a finite game, for any fixed opponents’ $$mix σ−i\sigma_{-i},$$ the set $$BRi(σ−i)⊂Δ(Si)BR_i(\sigma_{-i})\subset \Delta(S_i)$$ is convex.  
**Why.** Expected payoff is linear in$$ σi\sigma_i.$$ If $$σa,σb\sigma^a,\sigma^b$$ maximize it, so does any $$λσa+(1−λ)σb\lambda\sigma^a+(1-\lambda)\sigma^b.$$

### B. UHC of best-response

Sketch using **Berge’s Maximum Theorem**: action simplices compact; payoffs continuous in $$(σi,σ−i)(\sigma_i,\sigma_{-i});$$ feasible set doesn’t change with $$σ−i\sigma_{-i}.$$ ⇒ argmax is non-empty, compact-valued, UHC.

### C. LP for a 2×2 zero-sum

$$A=(2001)A=\begin{pmatrix}2&0\\0&1\end{pmatrix}.$$ Solve for row player. 
Primal:

$$max⁡p1,p2,vvs.t. 2p1+0p2≥v,  0p1+1p2≥v,  p1+p2=1,  p≥0.\max_{p_1,p_2,v} v\quad \text{s.t. } 2p_1+0p_2\ge v,\; 0p_1+1p_2\ge v,\; p_1+p_2=1,\; p\ge 0.$$

Binding equalization on support ⇒$$ 2p1=p2=v2p_1 = p_2 = v, p1+p2=1p_1+p_2=1.  
So p2=2p1p_2=2p_1 and 3p1=1⇒p=(13,23)3p_1=1\Rightarrow p=(\tfrac13,\tfrac{2}{3}), value v=23v=\tfrac{2}{3}.$$
Column’s dual gives $$q=(23,13)q=(\tfrac23,\tfrac13).$$

### D. KKT toy

$$min⁡x12∥x∥2\min_x \tfrac12\|x\|^2 s.t. a⊤x=ba^\top x=b.  
Lagrangian L=12x⊤x+λ(a⊤x−b)L=\tfrac12 x^\top x + \lambda(a^\top x - b).  
Stationarity ⇒ x∗=−λ∗ax^\ast = -\lambda^\ast a. Feasibility ⇒ a⊤x∗=b⇒−λ∗∥a∥2=b⇒λ∗=−b/∥a∥2a^\top x^\ast=b\Rightarrow -\lambda^\ast \|a\|^2=b\Rightarrow \lambda^\ast=-b/\|a\|^2.  
Thus x∗=(b/∥a∥2)ax^\ast = (b/\|a\|^2)a (orthogonal projection).$$

### E. Envelope single-parameter

$$V(θ)=max⁡q∈[0,1]θq−c(q)V(\theta)=\max_{q\in[0,1]} \theta q - c(q), cc convex, q∗(θ)q^\ast(\theta) unique.  
Envelope ⇒ V′(θ)=q∗(θ)V'(\theta)=q^\ast(\theta).  
Integrate: V(θ)=V(θ‾)+∫θ‾θq∗(s)dsV(\theta)=V(\underline\theta)+\int_{\underline\theta}^{\theta} q^\ast(s)ds.  
Payment in a mechanism: t(θ)=θq∗(θ)−V(θ)t(\theta)=\theta q^\ast(\theta)-V(\theta).$$

### F. Bayes update check

With the numbers in $$§4.2, verify Pr⁡(+)=0.46\Pr(+)=0.46 and tower law: 0.6087⋅0.46+0.2222⋅0.54=0.40.6087\cdot 0.46 + 0.2222\cdot 0.54 = 0.4.$$

---

# 8) Minimal “what to memorize”

- **Kakutani conditions:** domain compact convex; non-empty, convex, compact values; UHC.
    
- **LP dual templates** and **complementary slackness**.
    
- **Envelope formulas**:$$ V′(θ)=∂θf(x∗,θ)V'(\theta)=\partial_\theta f(x^\ast,\theta); $$payments via integral of allocation.
    
- **Bayes + tower:** posteriors, odds form, $$E[E[X∣Y]]=E[X]\mathbb{E}[\mathbb{E}[X\mid Y]]= \mathbb{E}[X].$$
    

---

# 9) Tiny coding tasks (optional, 20–30 min each)

- **LP solver for zero-sum games:** take AA, build the primal/dual, solve with any LP library; verify complementary slackness by checking equalized payoffs on the support.
    
- **Envelope demo:** pick $$c(q)=12αq2c(q)=\tfrac12 \alpha q^2;$$ compute $$q∗(θ)=max⁡{0,min⁡{1,θ/α}}q^\ast(\theta)=\max\{0,\min\{1,\theta/\alpha\}\}, then V(θ)V(\theta)$$ by integrating $$q∗q^\ast.$$
    

If you want, say the word and I’ll drop in clean Python for both.

---

# 10) How to study this in ~2–3 hours

1. Read §1–2 once, slowly (40–50 min).
    
2. Work Exercises A–C (30–40 min).
    
3. Read §3–4 (30–40 min).
    
4. Work D–F (20–30 min).
    
5. Optional coding (30–60 min).
    

---

If this pace feels good, I’ll queue **Week 1** next: normal-form games, dominance, Nash in 2×2, and solving a zero-sum with your LP tool.