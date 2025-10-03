#math #game_theory #book 


Awesome—let’s turn the bibliography into a **working toolkit**. Below I’ll tell you **what each book is for**, **how to read it fast**, **which chapters map to our weeks**, **what proofs/exercises to actually do**, and a **notation crib sheet** so you don’t get lost switching texts.

---

# 0) The core stack at a glance (choose 2–3 to read cover-to-cover)

|Role|Best pick|Why|When to use|
|---|---|---|---|
|Gentle but rigorous start|**Osborne — _An Introduction to Game Theory_**|Clear definitions, lots of worked examples; ideal for Week 1–2|First pass for foundations; quick lookups later|
|Mechanism-first, clean math|**Myerson — _Game Theory: Analysis of Conflict_**|Revelation, envelope/payment identity, auctions done right|Weeks 3–4; anytime you need IC/IR cleanly|
|Deep reference on dynamics|**Fudenberg & Tirole — _Game Theory_**|Subgame perfection, reputation, repeated games|Week 2 & 5; when you want the “full” proofs|
|Auctions specialization|**Krishna — _Auction Theory_**|One-stop for FPA/SPA, revenue equivalence, reserves, multi-unit|Week 4; then for deal-room calculations|
|Computation/PoA|**Nisan et al. — _Algorithmic Game Theory_**|PPAD, Lemke–Howson, smoothness/PoA; congestion games|Week 6 (and parts of Week 5)|
|Differential games|**Başar & Olsder — _Dynamic Noncooperative Game Theory_**|HJI/HJB, feedback Nash in continuous time|Week 6 (HJI quick start)|
|Mean-field games|**Carmona & Delarue — _Mean Field Games_ (2 vols.)**|HJB–Fokker–Planck systems, existence/uniqueness|Week 6 (MFG backbone)|
|Multiagent lens|**Shoham & Leyton-Brown — _Multiagent Systems_**|Correlated eq., learning, distributed mechanisms|Optional extension to Weeks 5–6|

> If you only do **three**: Osborne → Myerson → Nisan. Add Krishna for auctions depth, and Fudenberg–Tirole for repeated games.

---

# 1) How to read each book efficiently

## Osborne — _An Introduction to Game Theory_ (foundations, fast)

**Read for**: clean setup, best-response geometry, existence via Kakutani, and a LOT of examples you can solve by hand.

- **Week 1**: Normal form, dominance, mixed NE, **2×2 playbook** (solve several matrices).
    
- **Week 2**: Extensive form, info sets, **subgame perfection**; do backward-induction trees.
    
- **Week 3**: Bayesian games (Harsanyi types) — enough to set up BNE.
    
- **Exercises to actually do**:
    
    1. Iterated deletion (strict vs weak) on 2×3 and 3×3 games.
        
    2. Compute a mixed NE in a 2×22\times 2 where no pure NE exists (equalize payoffs).
        
    3. One credible-threat counterexample: find an NE that’s not SPNE, and fix it.
        
- **Why keep it nearby**: It’s the fastest way to sanity-check definitions and run hand computations.
    

## Myerson — _Game Theory: Analysis of Conflict_ (mechanism design spine)

**Read for**: direct mechanisms, **revelation principle**, **monotonicity + envelope**, and **payment identity**. The writing is surgical—perfect for quant mechanism work.

- **Week 3–4**:
    
    - BIC/DSIC definitions, envelope theorem in single-parameter environments.
        
    - **Payment identity** $$t(v)=v q(v)−∫qt(v)=v\,q(v)-\int q$$ and **revenue equivalence**.
        
    - Regularity and **virtual values** $$ϕ(v)=v−1−Ff\phi(v)=v-\frac{1-F}{f}.$$
        
- **Exercises to do**:
    
    1. Prove monotonicity of $$q(v)q(v)$$ under IC.
        
    2. Derive the payment identity from envelope + IR at the lowest type.
        
    3. Show that setting the reserve by $$ϕ(r∗)=0\phi(r^\ast)=0$$ maximizes expected revenue (single item, regular case).
        
- **Tip**: When stuck, write the **interim utility** u(v)u(v) and differentiate: $$u′(v)=q(v)u'(v)=q(v). $$Most payment formulas fall out in 5 lines.
    

## Fudenberg & Tirole — _Game Theory_ (deep dynamic + repeated)

**Read for**: **subgame perfection** proofs, **one-deviation principle**, reputation, and **Folk theorems**.

- **Week 2**: Sequential rationality & belief systems; **PBE** structure.
    
- **Week 5**: Repeated games; **Grim trigger** and finite punishments; formal Folk theorems.
    
- **Exercises to do**:
    
    1. Verify SPNE via one-deviation principle on a two-stage entry game.
        
    2. Derive the discount threshold $$δ≥uD−uCuD−vP\delta\ge\frac{u^D-u^C}{u^D-v^P}$$ for grim and compare to finite LL punishments.
        
- **Why use it**: When you want the fully rigorous dynamic arguments behind the quick recipes we used.
    

## Krishna — _Auction Theory_ (all the economics of auctions you actually need)

**Read for**: **FPA/SPA derivations**, order statistics, **affiliation**, and practical revenue comparisons. It’s the standard auction desk reference.

- **Week 4**:
    
    - IPV: **First-price bidding** via envelope (general FF and uniform closed form).
        
    - **Second-price** truthfulness proof; revenue equivalence conditions and failures.
        
    - **Reserves & optimal auctions** (ties back to Myerson).
        
- **Exercises to do**:
    
    1. Derive $$b(v)=v−∫0vF(s)n−1dsF(v)n−1b(v)=v-\frac{\int_0^v F(s)^{n-1}ds}{F(v)^{n-1}}.$$
        
    2. Compute the optimal reserve $$r∗r^\ast$$ for uniform and compare revenue.
        
    3. Show how risk aversion breaks RE (qualitatively and with a simple CRRA spec).
        

## Nisan, Roughgarden, Tardos, Vazirani — _Algorithmic Game Theory_ (AGT)

**Read for**: **computational** aspects (PPAD, Lemke–Howson), **Price of Anarchy** via the **smoothness** framework, and congestion games.

- **Week 6 (+ Week 5)**:
    
    - **Bimatrix NE** complexity; **Lemke–Howson** intuition.
        
    - **Congestion games** and Rosenthal potential.
        
    - **PoA bounds** and smoothness (transferable to mechanism tweaks).
        
- **Exercises to do**:
    
    1. Work a smoothness-style PoA bound for affine latency routing.
        
    2. Hand-execute a tiny Lemke–Howson path on a $$3×33\times 3$$ game.
        
    3. Build a potential function for a small congestion instance and show improvement paths terminate.
        

## Başar & Olsder — _Dynamic Noncooperative Game Theory_

**Read for**: **differential games**, **HJI/HJB**, feedback Nash in continuous time. It gives you the **Isaacs** toolbox you need for control-meets-games.

- **Week 6**:
    
    - Zero-sum HJI: $$−Vt=min⁡umax⁡v{ℓ+∇V⋅f} -V_t = \min_u \max_v \{\ell + \nabla V\cdot f\}.$$
        
    - Feedback strategies & verification theorems.
        
- **Exercises to do**:
    
    1. Solve a linear-quadratic two-player game (algebraic Riccati).
        
    2. Check the Isaacs condition on a simple min-max example (swap min/max and compare).
        

## Carmona & Delarue — _Mean Field Games_ (Vol. I–II)

**Read for**: **HJB–Fokker–Planck** systems, existence/uniqueness, and linear-quadratic MFGs (execution/crowding models).

- **Week 6**:
    
    - Deterministic & stochastic MFG systems
        
    - LQ-MFG: coupled Riccati + fixed-point on the mean flow
        
- **Exercises to do**:
    
    1. Derive the LQ-MFG equilibrium controls and population trajectory.
        
    2. Verify a fixed-point condition on the mean inventory path.
        

## Shoham & Leyton-Brown — _Multiagent Systems_

**Read for**: **correlated equilibrium**, learning dynamics, and mechanism basics from a CS lens.

- **Weeks 5–6 (optional)**:
    
    - CE/CCE interpretations with signals; links to no-regret learning.
        
    - Basic distributed mechanism ideas (good complement to Nisan).
        

---

# 2) Mapping chapters to our 6-week plan

- **Week 0 (tools):** Osborne prelims (convexity, best responses), quick Myerson intro to envelope; AGT’s PoA overview (intuition only).
    
- **Week 1 (normal form):** Osborne’s normal-form chapters + mixed NE; do several 2×22\times2 and 2×32\times3 by hand.
    
- **Week 2 (extensive):** Osborne for BI/SPNE → Fudenberg–Tirole for one-deviation and belief systems.
    
- **Week 3 (Bayesian):** Osborne’s Bayesian intro → Myerson for truthful mechanisms as “Bayesian games in disguise.”
    
- **Week 4 (mechanisms/auctions):** Myerson (revelation, envelope) → Krishna (FPA/SPA, reserves) → tie back to Myerson virtual values.
    
- **Week 5 (repeated/learning):** Fudenberg–Tirole (Folk theorems) → AGT (CE/CCE via regret).
    
- **Week 6 (computation/dynamics/quant):** AGT (PPAD, PoA, congestion) → Başar & Olsder (HJI) → Carmona & Delarue (MFG).
    

---

# 3) Proof templates to internalize (one page per template in Obsidian)

1. **Nash existence (finite games)**
    
    - Domain $$K=∏iΔ(Si)K=\prod_i \Delta(S_i)$$$ is compact/convex; expected payoffs continuous; by Berge, $$BRiBR_i$$$ is non-empty, convex, UHC; apply **Kakutani**.
        
2. **Payment identity (single-parameter IC)**
    
    - Truthful interim utility $$u(v)=v q(v)−t(v)u(v)=v\,q(v)-t(v); IC ⇒ u′(v)=q(v)u'(v)=q(v);$$ integrate with $$u(v‾)=0u(\underline v)=0.$$
        
3. **First-price bidding (symmetric IPV)**
    
    - $$u′(v)=F(v)n−1u'(v)=F(v)^{n-1}; equate with F(v)n−1(v−b(v))F(v)^{n-1}(v-b(v)); solve for b(v)b(v).$$
        
4. **Grim-trigger IC (repeated)**
    
    - Compare $$VC=uC1−δV_C=\frac{u^C}{1-\delta} vs VD=uD+δvP1−δV_D=u^D+\frac{\delta v^P}{1-\delta}; get δ≥uD−uCuD−vP\delta\ge\frac{u^D-u^C}{u^D-v^P}.$$
        
5. **PoA via smoothness** (AGT)
    
    - Show $$∑iui(si∗,s−i)≥λ⋅OPT−μ⋅Cost(s)\sum_i u_i(s_i^\ast, s_{-i}) \ge \lambda \cdot \text{OPT} - \mu \cdot \text{Cost}(s); conclude PoA≤μλ\text{PoA}\le\frac{\mu}{\lambda}.$$
        

---

# 4) Notation crib sheet (switching safely between books)

| Concept         | Osborne                          | Myerson                                | Fudenberg–Tirole | Krishna                 | AGT                         |
| --------------- | -------------------------------- | -------------------------------------- | ---------------- | ----------------------- | --------------------------- |
| Action/strategy | $$sis_i, σi\sigma_i$$            | $$sis_i, truthful report v^i\hat v_i$$ | $$sis_i$$        | $$bid bib_i$$           | $$sis_i, x,yx,y$$ for mixes |
| Type/value      | $$— / θi\theta_i (Bayes chap.)$$ | $$θi\theta_i or viv_i$$                | $$θi\theta_i$$   | viv_i                   | $$v,θv,\theta$$ as needed   |
| Payoff          | $$ui(⋅)u_i(\cdot)$$              | $$uiu_i, interim u(v)u(v)$$            | $$uiu_i$$        | surplus/revenue         | utility/cost                |
| Allocation      | —                                | $$qi(⋅)∈[0,1]q_i(\cdot)\in[0,1]$$      | —                | winner indicator        | flow/route                  |
| Payment         | —                                | $$ti(⋅)t_i(\cdot)$$                    | —                | price/bid payment       | cost/latency                |
| Mixed           | $$σi\sigma_i$$                   | —                                      | σi\sigma_i       | distributions over bids | probability vectors x,yx,y  |
| Beliefs         | $$μ(⋅)\mu(\cdot)$$               | posterior over types                   | belief systems   | order stats             | priors/posteriors as needed |

**Tip**: In mechanisms, always write **interim** objects: $$q(v)=E[Q∣v]q(v)=\mathbb{E}[Q\mid v], t(v)=E[T∣v]t(v)=\mathbb{E}[T\mid v].$$

---

# 5) Minimal problem set (one evening per row)

|Topic|Do this (from the books)|What you’re checking|
|---|---|---|
|Mixed NE 2×22\times2|Osborne: a no-pure-NE matrix; compute the mixed using indifference|You can equalize payoffs and sanity-check supports|
|SPNE vs NE|Osborne/F&T: construct a non-credible threat; then refine to SPNE|You can use one-deviation logic|
|BNE cutoff|Osborne: 2-type entry or uniform cost entry|You can condition on your type and solve a fixed point|
|Payment identity|Myerson: derive t(v)t(v) for a monotone q(v)q(v)|Envelope is internalized|
|FPA derivation|Krishna: derive b(⋅)b(\cdot) for general FF; check uniform|You can do the envelope + inversion|
|Folk theorem IC|F&T: compute δ\delta for grim and finite-LL|You can design credible punishments|
|PoA bound|AGT: affine congestion bound (Pigou)|You can relate equilibrium to optimum|
|LQ-MFG sketch|Carmona–Delarue: write the coupled Riccati system|You can set up HJB–FP structure|

---

# 6) Time-boxed reading plans

**Sprint (2 weeks, ~2–3 h/day)**

- Days 1–3: Osborne Wk1–2 chapters + hand exercises.
    
- Days 4–6: Myerson core (revelation, envelope); do payment identity once.
    
- Days 7–8: Krishna FPA/SPA + reserves; one uniform derivation + reserve calc.
    
- Days 9–10: Fudenberg–Tirole repeated games; grim vs finite punishments.
    
- Days 11–12: AGT smoothness & congestion; one PoA proof.
    
- Days 13–14: Kyle note + Başar/Olsder HJI quick derivation; Carmona LQ-MFG overview.
    

**Deep (6 weeks, aligned with our course)**  
Read the same mapping as §2, but **work 2–3 end-of-chapter problems** per week and implement 1 code mini-project alongside.

---

# 7) Common sticking points & how to resolve them fast

- **“Which expectation?”** If it’s mechanisms, default to **interim** (condition on your type, integrate others).
    
- **“Is my threat credible?”** Try one-deviation at the subgame where the threat is executed. If it fails there, it’s not SPNE.
    
- **“Why payments look like integrals?”** Re-derive envelope: u′(v)=q(v)u'(v)=q(v); integrate and subtract from v q(v)v\,q(v).
    
- **“PoA proof feels magical.”** Write the smoothness inequality and add across players; most bounds become 5 lines.
    

---

# 8) What to capture in Obsidian (templates)

- **Definition block** (one per concept): short statement + a 2-line example.
    
- **Proof template**: key identities (Kakutani hypotheses, envelope step, grim IC).
    
- **Recipe card**: e.g., “FPA derivation in 4 lines,” “Mixed 2×22\times2 in 3 lines.”
    
- **Cross-refs**: link Osborne→Myerson (payment identity), Myerson→Krishna (reserves), AGT→Week 5 (CE/CCE vs regret).
    

---

**Your turn (one quick question):**  
Do you want the **Sprint (2-week)** or **Deep (6-week)** reading plan?  
(Once you pick, I’ll lay out the exact daily reading + the specific problems to attempt each day.)
