#math #game_theory #quantitative_finance

Awesome—Week 4 is the payoff: **mechanism design + auctions**. We’ll build everything from the revelation principle and the envelope/payment identity, then derive first-price strategies, show why second-price is truthful, and finish with **Myerson’s virtual values** (optimal reserves). I’ll keep it tight, proof-sketchy, and quant-useful.

---

# Week 4 — Mechanism Design & Auctions (IPV focus)

## 0) Learning goals

By the end you should be able to:

1. State and use the **revelation principle** to work with direct, truthful mechanisms.
    
2. Apply **monotonicity + envelope** to get the **payment identity** $$t(v)=v q(v)−∫0vq(s) ds+constt(v)=v\,q(v)-\int_0^v q(s)\,ds + \text{const}.$$
    
3. Derive **first-price** bidding $$b(⋅)b(\cdot)$$ for $$i.i.d. IPV$$ bidders, general FF, and the uniform closed form.
    
4. Explain **revenue equivalence** and when it breaks.
    
5. Compute **Myerson’s optimal reserve** via virtual values $$ϕ(v)=v−1−F(v)f(v)\phi(v)=v-\frac{1-F(v)}{f(v)}.$$
    

---

## 1) Setup & language (single-parameter IPV)

- nn risk-neutral bidders, values i.i.d. $$vi∼Fv_i\sim F on [0,vˉ][0,\bar v]$$ with density $$f>0f>0.$$
    
- A (direct) mechanism asks each ii to **report** $$v^i\hat v_i.$$ It chooses allocation $$qi(v^)∈[0,1]q_i(\hat v)\in[0,1]$$ (prob. bidder ii gets the item) and payment $$ti(v^)t_i(\hat v).$$
    
- Bidder utility (quasi-linear): $$ui(vi;v^)=vi qi(v^)−ti(v^)u_i(v_i;\hat v)=v_i\,q_i(\hat v)-t_i(\hat v).$$
    

**Truthfulness & feasibility**

- **IC (dominant-strategy)** or **Bayesian IC (BIC)** depending on the environment.
    
- **IR:** $$ui(vi;truth)≥0u_i(v_i;\text{truth})\ge 0$$ for participating types.
    
- **Feasibility:** $$∑iqi(v^)≤1\sum_i q_i(\hat v)\le 1.$$
    

---

## 2) Revelation principle (work in the truthful world)

Any equilibrium outcome from any (possibly complicated) mechanism can be replicated by a **truthful direct mechanism** with the same equilibrium allocation and payments.  
**Why it’s gold:** you analyze **allocation rules** $$q(⋅)q(\cdot)$$ and deduce payments from **envelope**, instead of chasing strategic bids.

---

## 3) Monotonicity + envelope ⇒ the payment identity

In single-parameter IPV with risk-neutral bidders:

- **Monotonicity:** In any IC mechanism, the interim allocation probability q(v)q(v) must be **non-decreasing** in vv. (Higher value shouldn’t decrease your chance to win.)
    
- **Envelope theorem:** Let $$u(v)u(v)$$ be the truthful interim utility. Then
    
    $$u′(v)=q(v)⇒u(v)=u(v‾)+∫v‾vq(s) ds.u'(v)=q(v)\quad\Rightarrow\quad u(v)=u(\underline v)+\int_{\underline v}^{v} q(s)\,ds.$$
- Because $$u(v)=v q(v)−E[t(v)]u(v)=v\,q(v)-\mathbb{E}[t(v)],$$ rearrange to get the **payment identity**:
    
     $$t(v)=v q(v)−∫v‾vq(s) ds+C \boxed{\,t(v)=v\,q(v)-\int_{\underline v}^{v} q(s)\,ds + C\,}$$
    
    where $$ C=u(v‾)C=u(\underline v).$$ With IR normalized at the lowest type $$u(v‾)=0u(\underline v)=0, **C=0C=0**.$$
    

**Interpretation:** Once you choose a monotone allocation q(⋅)q(\cdot), payments are pinned down up to a constant (the lowest type’s rent).

---

## 4) Auction archetypes through the lens above

### 4.1 Second-price (Vickrey) auction (IPV, private values)

- Allocation: highest report wins $$(q(v)=Pr⁡{v is the max}q(v) = \Pr\{v \text{ is the max}\})$$, payments: the **second-highest** bid.
    
- **Truthful bidding** $$b(v)=vb(v)=v$$ is a **dominant strategy**: your bid only affects _whether_ you win, not the price you pay _if_ you win; hence you want to win exactly when vv exceeds the (unknown) second-highest value.
    
- Revenue via payment identity matches other standard formats under the RE conditions below.
    

### 4.2 First-price sealed-bid (FPA), symmetric BNE

Assume all others use a strictly increasing bidding function $$b(⋅)b(\cdot)$$. Type vv wins iff all rivals’ values ≤v\le v, so **win probability**

$$q(v)=F(v) n−1.q(v)=F(v)^{\,n-1}.$$

Expected utility when bidding according to $$b(v)b(v):$$

$$u(v)=q(v) (v−b(v)).u(v)=q(v)\,(v-b(v)).$$

**Envelope route (cleanest):**

$$u′(v)=q(v)=F(v)n−1,u(0)=0 ⇒ u(v)=∫0vF(s)n−1ds.u'(v)=q(v)=F(v)^{n-1},\quad u(0)=0 \ \Rightarrow\ u(v)=\int_0^{v} F(s)^{n-1} ds.$$

Equate with $$q(v)(v−b(v))q(v)(v-b(v)):$$

$$F(v)n−1(v−b(v))=∫0vF(s)n−1dsF(v)^{n-1}\big(v-b(v)\big)=\int_0^{v} F(s)^{n-1} ds ⇒ b(v)=v−∫0vF(s)n−1 dsF(v)n−1 .\Rightarrow\quad \boxed{\,b(v)=v-\frac{\displaystyle\int_0^{v} F(s)^{n-1}\,ds}{F(v)^{n-1}}\,}.$$

**Uniform[0,1]**: $$∫0vsn−1ds=vnn⇒b(v)=v−vn/nvn−1=n−1nv\int_0^{v} s^{n-1}ds=\frac{v^n}{n}\Rightarrow b(v)=v-\frac{v^n/n}{v^{n-1}}=\frac{n-1}{n}v.$$

**Notes**

- **Risk aversion** pushes bids **up** in FPA (but not in SPA).
    
- Correlation/interdependence break the simple $$F(v)n−1F(v)^{n-1}$$ win-probability logic.
    

### 4.3 Dutch = First-price; English ≈ Second-price (IPV)

- Dutch (clock descends; first to stop wins and pays clock) is **strategically equivalent** to first-price.
    
- English (ascending) with private values induces the same outcome as second-price (the last dropout before you pins your price).
    

### 4.4 All-pay (everyone pays their bid)

With IPV and symmetric BNE, allocation still to highest bid/value; expected revenue equals first-price/second-price under RE assumptions (see next).

---

## 5) Revenue Equivalence (RE)

**Theorem (sketch).** Under: (i) i.i.d. IPV, (ii) risk-neutral bidders, (iii) symmetric mechanisms that **allocate to the highest value** with the same $$q(⋅)q(\cdot),$$ and (iv) the **lowest type’s rent** is the same (often normalized to 0), then **all such auctions yield the same expected revenue** and the same interim utilities.

**Why:** Payment identity ties revenue to $${q(⋅),u(v‾)}\{q(\cdot),u(\underline v)\}$$ only. If those match, revenues match—irrespective of pricing rule details.

**When RE fails:** risk aversion, budget constraints, asymmetric distributions, reserve prices (different qq), entry costs, interdependent/common values, or different bottom-type rents.

---

## 6) Myerson’s Lemma & Virtual Values (optimal auctions)

- **Virtual value:** $$ϕ(v)=v−1−F(v)f(v).\displaystyle \phi(v)=v-\frac{1-F(v)}{f(v)}.$$
    
- **Myerson (single item, IPV, regular case $$ϕ↑\phi\uparrow):$$** The revenue-optimal truthful mechanism maximizes **expected virtual surplus**: allocate to argmax $$ϕ(vi)\phi(v_i)$$ **only if** that max is **non-negative**; payments come from the payment identity.
    

**Equivalent operational rule:**

- Set a **reserve price** $$r∗r^\ast such that ϕ(r∗)=0\phi(r^\ast)=0 (i.e., r∗r^\ast solves r∗=1−F(r∗)f(r∗)r^\ast=\frac{1-F(r^\ast)}{f(r^\ast)} + r∗r^\ast $$rearranged) and sell to the highest bidder only if their value exceeds $$r∗r^\ast.$$
    
- **Uniform[0,1]**: $$ϕ(v)=2v−1⇒r∗=12\phi(v)=2v-1\Rightarrow r^\ast=\tfrac12.$$
    
- With i.i.d. bidders, the **optimal auction** is a **second-price with reserve $$r∗r^\ast$$** (or, equivalently, first-price with the same reserve), plus standard tie/zero-rent conventions.
    

**Irregular FF** (non-monotone $$\phi$$): “**iron**” $$\phi$$ (convexify its revenue curve) to get a monotone “ironed virtual value”; allocate using that—details matter in practice (e.g., heavy-tailed priors).

---

## 7) Comparative statics & variants (one-liners)

- **More bidders** ⇒ FPA bids rise; revenue increases in nn (under RE conditions).
    
- **Risk aversion** ⇒ FPA bids increase; SPA remains truthful but **revenues diverge** (RE fails).
    
- **Affiliation/interdependence** ⇒ ascending auctions can dominate sealed-bid on revenue due to information release.
    
- **Multi-unit** (k units): **VCG/Uniform-price** mechanisms generalize DSIC/efficiency (beyond Week 4 scope, but similar envelope logic).
    

---

## 8) Quant/microstructure lens

- **Maker–taker fees/reserves** ≈ reserve pricing: screen out low-quality order flow to raise expected revenue (or reduce adverse selection).
    
- **Order-flow auctions** (e.g., payment for order flow): mechanism design of routing + rebates to maximize “virtual surplus” under broker types.
    
- **Spoof-detection mechanisms:** IC constraints for truthful reporting of intent; payment identity guides penalty schedules consistent with participation.
    

---

## 9) Pitfalls to avoid

- Using envelope without **monotonicity** or without pinning u(v‾)u(\underline v).
    
- Confusing **ex post** allocation with **interim** probability q(v)q(v).
    
- Assuming RE when reserves differ or bidders are risk-averse.
    
- Forgetting that FPA derivation **needs** strictly increasing symmetric strategies to invert $$b(⋅)b(\cdot).$$
    

---

## 10) Practice (short, surgical)

1. **Payment identity from allocation:** Suppose a truthful single-item mechanism with $$q(v)=F(v)n−1q(v)=F(v)^{n-1}$$ (highest-value allocation) and $$u(0)=0u(0)=0.$$ Write t(v).
    
2. **Uniform FPA:** With $$F(x)=xF(x)=x$$ on [0,1][0,1] and nn bidders, derive b(v).
    
3. **Optimal reserve:** For Uniform[0,1], compute $$r∗r^\ast$$ from $$ϕ(v)=2v−1\phi(v)=2v-1.$$ Compare expected revenue of SPA with and without that reserve (hint: use order statistics for a quick check).
    

(We can work these interactively—one step at a time—when you’re ready.)

---

## 11) Optional coding (1–2 h total)

- **FPA bidding function (general FF).** Numerically evaluate $$b(v)=v−∫0vF(s)n−1dsF(v)n−1b(v)=v-\frac{\int_0^v F(s)^{n-1}ds}{F(v)^{n-1}};$$ test $$F=F= Beta(α,β\alpha,\beta).$$
    
- **Revenue equivalence sim.** Simulate SPA, FPA, and All-pay under IPV/Uniform; verify mean revenues match; then break RE (risk-averse CRRA utility) and observe divergence.
    
- **Myerson reserve.** Compute $$ϕ\phi,$$ solve $$ϕ(r∗)=0\phi(r^\ast)=0,$$ simulate SPA with reserve and compare revenue.
    

---

### Quick check (one step)

In the symmetric FPA with i.i.d. IPV values and strictly increasing strategies, what is the **win probability** q(v) for a bidder of type v in terms of F and n?  
(Just the expression—we’ll use it to build the envelope step next.)