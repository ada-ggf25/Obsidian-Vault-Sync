#math #game_theory #quantitative_finance

Awesome—Week 3 is where uncertainty enters the room. We’ll formalize **Bayesian games**, build the instinct for **type-contingent strategies**, and practice solving **Bayesian Nash equilibria (BNE)** in small models you’ll reuse in auctions and microstructure.

---

# Week 3 — Incomplete Information (Bayesian Games & BNE)

## 1) What a Bayesian game is (Harsanyi view)

A **Bayesian game** is a tuple $$(N,{Si},{Θi},{ui},μ)(N,\{S_i\},\{\Theta_i\},\{u_i\},\mu):$$

- Players $$i∈Ni\in N.$$
    
- Action sets $$SiS_i$$ (finite or compact).
    
- **Types** $$Θi\Theta_i$$ (private information: values, costs, signals).
    
- Payoffs $$ui(s,θ)u_i(s,\theta)$$ depend on **actions** $$s=(si,s−i)s=(s_i,s_{-i})$$ and **types** $$θ=(θi,θ−i)\theta=(\theta_i,\theta_{-i}).$$
    
- A **common prior** $$μ\mu on Θ=∏iΘi\Theta=\prod_i \Theta_i$$ (often independent across players, but can be correlated).
    

**Harsanyi transformation:** add Nature who draws $$θ∼μ\theta\sim \mu,$$ reveals $$θi\theta_i$$ only to player ii, then players choose actions (possibly mixed).  
A **strategy** is now $$σi:Θi→Δ(Si) \sigma_i:\Theta_i \to \Delta(S_i):$$ “what I do for every type I might be.”

Three expectation layers to keep straight:

- **Ex ante**: before learning any type.
    
- **Interim**: after observing your own type θi\theta_i but before others’.
    
- **Ex post**: after all types are known.
    

---

## 2) Bayesian Nash equilibrium (BNE)

A profile $$σ⋆=(σi⋆)i\sigma^\star = (\sigma_i^\star)_{i}$$ is a **BNE** if for each player ii and each type $$θi\theta_i,

σi⋆(θi)∈arg⁡max⁡σi(θi)Eθ−i∼μ(⋅∣θi)[ ui(σi(θi),σ−i⋆(θ−i), θi,θ−i) ].\sigma_i^\star(\theta_i) \in \arg\max_{\sigma_i(\theta_i)} \mathbb{E}_{\theta_{-i}\sim \mu(\cdot \mid \theta_i)} \big[\,u_i\big(\sigma_i(\theta_i),\sigma_{-i}^\star(\theta_{-i}),\,\theta_i,\theta_{-i}\big)\,\big].$$

Interpretation: holding beliefs (from the common prior via Bayes) and others’ type-strategies fixed, every type best-responds.

**Why this matters:** Most mechanism/auction equilibria are exactly BNE. Microstructure pricing rules (Kyle) are interim best responses under beliefs about order flow.

---

## 3) Two canonical information structures

- **Independent private values (IPV):** $$θi∼iidF \theta_i \stackrel{\text{iid}}{\sim} F;$$ each player’s payoff depends on own value and actions (auctions, matching).
    
- **Common (or interdependent) values:** Payoff depends on a common primitive vv; types/signals are noisy measurements → **winner’s curse** corrections (bid shading increases with conditional inference from “I won”).
    

**Affiliation** (positive dependence) and **monotone likelihood ratio** often imply **monotone** (cutoff) strategies and facilitate existence/uniqueness via lattice/fixed-point tools.

---

## 4) How to solve small Bayesian games (playbooks)

### A) **Cutoff (threshold) strategies** for binary actions

Structure: each player chooses $$a∈{0,1}a\in\{0,1\}$$ (e.g., **Enter** vs **Stay out**) with cost or value θi\theta_i.  
Guess a **symmetric cutoff**: player ii chooses a=1a=1 if $$θi≤t\theta_i \le t.$$ 
Steps:

1. Given opponent’s cutoff tt, compute the **interim expected payoff** from a=1a=1 for type $$θi\theta_i.$$
    
2. Define the **indifference type** $$θ†(t) \theta^\dagger(t)$$ that makes the player exactly indifferent.
    
3. **Fixed point:** in symmetric equilibrium $$t=θ†(t) t = \theta^\dagger(t).$$
    

If $$θ∼Uniform[0,1] \theta \sim \text{Uniform}[0,1]$$ and actions are strategic substitutes (your gain from a=1a=1 is higher when the other _doesn’t_ choose 1), the mapping is usually a **contraction** → unique cutoff.

---

### B) **Two-type discrete games** (quick BNE by cases)

Each player has type $$HH or LL with Pr⁡(H)=p\Pr(H)=p.$$ Write best responses for each type against each _type-contingent_ opponent strategy. Enumerate a few strategy profiles $$(e.g., H→a,L→bH\to a, L\to b)$$ and check best-response conditions and Bayes beliefs. This is the cleanest way to “feel” BNE before calculus shows up.

---

### C) **Auctions, briefly previewed**

- **Second-price (Vickrey), IPV:** truthful bidding $$b(v)=vb(v)=v$$ is a **dominant strategy** (beyond BNE).
    
- **First-price, IPV (risk neutral):** symmetric BNE is **strictly increasing**; with nn i.i.d. $$U[0,1]U[0,1], b(v)=n−1nvb(v)=\frac{n-1}{n}v.$$ (We’ll derive this fully in Week 4 via the envelope approach.)
    
- **Common values:** conditional on winning, the expected value **falls** (winner’s curse), so equilibrium bids shade more than in IPV; signals and affiliation shape the correction.
    

---

## 5) Worked templates (keep for reference)

### Template 1 — Entry with private costs (cutoff equilibrium)

Two firms $$i=1,2i=1,2.$$ Each draws cost $$ci∼U[0,1]c_i \sim U[0,1]$$ i.i.d. Actions: **Enter** (EE) or **Stay out** (OO). Payoffs:

- If you enter **alone**: $$π=B−ci \pi = B - c_i (e.g., B=1B=1).$$
    
- If **both** enter: $$π=b−ci\pi = b - c_i$$ with $$0≤b<B0 \le b < B$$ (competition hurts; often b=0b=0).
    
- If you stay out: 00.
    

**Cutoff guess:** enter if $$ci≤tc_i \le t.$$ Then $$Pr⁡(other stays out)=1−F(t)=1−t\Pr(\text{other stays out}) = 1 - F(t) = 1-t.$$  
Interim expected profit from entering for type cc:

$$(1−t)⏟alone(B−c)+t⏟both(b−c).\underbrace{(1-t)}_{\text{alone}}(B-c) + \underbrace{t}_{\text{both}}(b-c).$$

Indifference at $$c=tc=t:$$

$$(1−t)(B−t)+t(b−t)=0.(1-t)(B-t) + t(b-t) = 0.$$

Solve the fixed point for tt (unique for standard $$B>bB>b).$$  
Special case $$B=1,b=0B=1, b=0:$$ equation reduces to $$(1−t)(1−t)+t(0−t)=0⇒1−2t=0⇒t=12(1-t)(1-t) + t(0-t)=0 \Rightarrow 1-2t=0 \Rightarrow t=\tfrac12.$$
(You’ll reuse this algebra constantly: compute profit **given others’ cutoff**, set **type = profit**, solve for tt.)

---

### Template 2 — Discrete 2×2 Bayesian coordination

Two players, types HH or LL with $$Pr⁡(H)=p\Pr(H)=p.$$ Actions $${A,B}\{A, B\}.$$ Payoffs reward **matching** but HH prefers AA, LL prefers BB:

- If both play AA: HH gets 22, LL gets 11.
    
- If both play BB: HH gets 11, LL gets 22.
    
- If mismatch: payoff 00.
    

Candidate $$BNE: H→AH\to A, L→BL\to B$$ for both players.  

Why plausible: each type’s **interim** best response is to choose the action that maximizes expected coordination **given** the distribution of the opponent’s type; because both types make opposite choices, each type sees the opponent match with probability pp or 1−p1-p appropriately, making the mapping self-consistent. (You can check by conditioning on your type and applying Bayes—no calculus.)

---

## 6) Efficiency language you’ll see

- **Ex post efficient**: action profile maximizes total surplus for **every** type profile $$\theta.$$ (Strong—often unattainable under private info and IC.)
    
- **Interim efficient**: no other mechanism makes every type strictly better **in expectation** given their own type (subject to IC).
    
- **Revenue equivalence** (Week 4): in IPV under standard regularity, any IC mechanism with the same allocation rule and zero rent for the lowest type yields the same expected payments.
    

---

## 7) Winner’s curse in one line

In common-value auctions, conditional on the event “I win,” others’ signals tend to be **low** → your posterior value **drops**. Correct bid = prior mean **minus** an information-based markdown. Failing to adjust → biased, loss-making wins.

---

## 8) Existence/uniqueness (what to remember)

- With compact actions, continuous payoffs, and measurable strategies, BNE exists (Harsanyi, via Kakutani) under mild conditions.
    
- **Monotone strategies** (cutoffs) and **single-crossing** help uniqueness and comparative statics (Topkis/Tarski tools): if higher types have _increasing_ incentives to choose higher actions, best responses are monotone and fixed points are easier.
    

---

## 9) Common pitfalls

- **Forgetting the type index.** Best responses are **type-by-type** decisions.
    
- **Mixing ex ante and interim payoffs.** Keep the lens explicit.
    
- **Using prior instead of posterior.** Condition on your type (interim) and integrate over others’ types with Bayes.
    
- **Assuming independence when there’s correlation.** Correlation changes beliefs and hence best responses (and can break simple cutoffs).
    

---

## 10) Quant connections

- **Market microstructure (Kyle 1985, one-period preview):** informed trader observes a signal $$\theta$$ about value $$v$$, chooses order $$x(θ)x(\theta);$$ market maker sets price $$p=E[v∣order flow]p=\mathbb{E}[v\mid \text{order flow}].$$ Linear BNE emerges under Gaussian primitives: both $$x(θ)x(\theta)$$ and pricing rule are linear (you’ll code this in Week 6).
    
- **Adverse selection & liquidity:** spreads compensate for the Bayesian loss to informed traders; equilibria reflect beliefs about the mix of informed/uninformed flow.
    
- **Mechanism intuition:** optimal fees/rebates often come from monotone allocations + envelope—your Week 4 toolkit.
    

---

## 11) Optional coding mini-tasks (1–2 h total)

1. **Cutoff solver:** implement a fixed-point loop for the entry game; show convergence to t⋆t^\star from different initials.
    
2. **Discrete BNE enumerator:** for small type/action sets, enumerate type-contingent strategies and check best-response inequalities under Bayes.
    
3. **Winner’s curse sim:** simulate a common-value auction with noisy signals; report the ex-post value conditional on winning vs unconditional.
    

---

## 12) Tiny practice (one step)

Consider the entry game in **Template 1** with B=1B=1 and b=0b=0, costs $$c∼U[0,1]c\sim U[0,1],$$ and a symmetric cutoff tt.  
**Question (single step):** For a player with type cc, write the **expected profit from entering** as a function of cc and tt. (Just the expression; we’ll use it to find the indifference condition next.)