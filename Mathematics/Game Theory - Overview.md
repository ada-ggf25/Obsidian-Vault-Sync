#math #game_theory #quantitative_finance

[[Game Theory - Foundations]]
[[Game Theory - Strategic-form Games]]
[[Game theory - Extensive form & Dynamic Consistency]]
[[Game Theory - Incomplete Information]]
[[Game Theory - Mechanism Design & Auctions]]
[[Game Theory - Repeated games & Learning]]
[[Game Theory  - Computational & Finance Applications]]

Awesome goal. With your physics background, you’ve already got 80% of the math you need—let’s channel it into a fast, rigorous path that’s useful for quant work.
# Crash-Course Roadmap (6 weeks, adjustable)

**Week 0 – Tools you already know (2–3h refresher)**

- Convexity & fixed points: Brouwer/Kakutani (what they say, not full proofs), upper hemicontinuity.
    
- Linear programs & duality; KKT; Envelope theorem (for mechanism design).
    
- Bayes’ rule & conditional expectations as “belief updates.”
    

**Week 1 – Strategic-form (normal-form) games**

- Best responses, dominance (strict/weak), iterated deletion.
    
- Nash equilibrium (NE): pure vs mixed; existence (Kakutani sketch).
    
- Zero-sum: minimax = LP duality; security levels; value of the game.
    
- _Deliverables:_ compute mixed NE for 2×2; solve a zero-sum matrix game via LP.
    

**Week 2 – Extensive form & dynamic consistency**

- Game trees, information sets, perfect/imperfect information.
    
- Subgame perfect equilibrium (SPNE), backward induction, one-deviation principle.
    
- Signaling & cheap talk (why talk can matter or not).
    
- _Deliverables:_ convert a tree to normal form; find SPNE; explain why SPNE ⊂ NE.
    

**Week 3 – Incomplete information (Bayesian games)**

- Harsanyi types; interim vs ex-post efficiency.
    
- Bayesian Nash equilibrium; “beliefs + strategies = equilibrium.”
    
- _Deliverables:_ solve a simple entry game with private costs; compute a BNE in a toy auction.
    

**Week 4 – Mechanism design & auctions (very quant-relevant)**

- Direct mechanisms, IC/IR, Revelation Principle.
    
- Single-parameter environments; monotonicity + envelope formula.
    
- Auction formats: FP/SPA/APA; revenue equivalence; reserve prices; Myerson virtual values.
    
- _Deliverables:_ derive bidder strategy in first-price auction with i.i.d. uniform types; compare revenues to second-price.
    

**Week 5 – Repeated games & learning**

- Folk theorems; trigger/Grim/Tit-for-Tat; discounting and enforceability.
    
- No-regret dynamics (MWU), fictitious play, convergence to (coarse) correlated equilibrium.
    
- Price of Anarchy; potential games; congestion games.
    
- _Deliverables:_ implement MWU and show regret → 0; compute PoA on a tiny congestion game.
    

**Week 6 – Computational & finance applications**

- Complexity: PPAD-hardness of NE; Lemke–Howson for bimatrix games.
    
- Evolutionary games & replicator dynamics (continuous-time intuition).
    
- Differential/mean-field games (quant links: execution, crowding, systemic effects).
    
- Market microstructure as games: Kyle (informed trader vs market maker), market-making spread as equilibrium, inventory costs; payment-for-order-flow as mechanism design.
    
- _Deliverables:_ simulate replicator dynamics on Rock-Paper-Scissors; code a basic Kyle-style one-period game and compare equilibria under parameter changes.
    

---

## Mental Models & “Why it matters for quant”

- **Equilibrium = fixed point of best responses.** Your convex analysis makes Kakutani feel natural.
    
- **Duality = zero-sum DNA.** Optimal market-making often reduces to dual problems (risk vs profit).
    
- **Mechanisms = exchanges.** Auctions ≈ limit-order markets; IC/IR shape fee schedules, reserves, rebates.
    
- **Learning in games = online learning.** MWU/no-regret connects directly to adversarial bandits and market adaptivity.
    
- **Price of Anarchy = slippage from decentralization.** Useful for multi-agent execution and liquidity games.
    

---

## Coding mini-projects (short, punchy)

1. **Solve a zero-sum game via LP** (scipy.optimize): return value & optimal mixed strategies.
    
2. **Mixed-NE finder for 2×2**: analytic formula + verification by best-response equalization.
    
3. **MWU Dynamics**: show time-average play approaches a coarse-correlated equilibrium; track regret.
    
4. **Replicator Dynamics**: ODE integrator for population strategies; visualize cycles in RPS.
    
5. **Toy first-price auction**: simulate bidding with risk-neutral i.i.d. types; estimate revenue; add reserve price.
    
6. **One-period Kyle model**: calibrate signals/variance; compute linear equilibrium; see how spreads move.
    

(If you want, I can drop in clean Python templates for any of these.)

---

## Key theorems & “one-liners” to remember

- **Nash Existence:** finite normal-form games have at least one mixed NE (Kakutani).
    
- **Minimax (von Neumann):** maxmin = minmax in finite zero-sum games.
    
- **Revelation Principle:** any implementable outcome can be implemented truthfully.
    
- **Revenue Equivalence:** under standard assumptions, symmetric auctions yield same expected revenue.
    
- **Folk Theorem:** with enough patience, many efficient outcomes are enforceable in repeated games.
    
- **Potential Games:** improvement paths converge; equilibria are local optima of a potential function.
    

---

## Suggested texts (use in this order)

- **Osborne** – _An Introduction to Game Theory_ (gentle but rigorous starter).
    
- **Myerson** – _Game Theory: Analysis of Conflict_ (clean, mechanism-design-savvy).
    
- **Fudenberg & Tirole** – _Game Theory_ (deep reference, dynamic games).
    
- **Krishna** – _Auction Theory_ (quant-friendly).
    
- **Nisan et al.** – _Algorithmic Game Theory_ (computational & PoA).
    
- **Başar & Olsder** (differential games), **Carmona & Delarue** (mean-field games), **Shoham & Leyton-Brown** (multiagent perspective).
    

---

## How we’ll study (interactive)

Each session we’ll:

1. introduce one concept with a crisp picture or equation,
    
2. work a **single** carefully chosen exercise together,
    
3. check understanding with a quick question,
    
4. note how it shows up in trading/microstructure.
    

If you want, I can turn this into a 2-week sprint by compressing deliverables and focusing on auctions + learning + microstructure.

**Quick first step:** how many hours per week can you realistically give this (e.g., 5h, 10h, 20h)?  
Your answer will let me size the plan and pick the first exercise accordingly.