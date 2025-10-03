#math #game_theory #quantitative_finance

Great—Week 1 is where games become concrete. We’ll lock in normal-form games, dominance, best responses, and Nash equilibrium (pure & mixed), plus a reliable 2×2 workflow you can actually use.

---

# Week 1 — Strategic (normal) form, dominance, best responses, Nash

## 1) Objects & notation (keep it uniform)

- **Players:** $$i=1,…,ni=1,\dots,n.$$
    
- **Actions:** finite sets $$SiS_i.$$ A **pure profile** is $$s=(s1,…,sn)∈∏iSis=(s_1,\dots,s_n)\in \prod_i S_i.$$
    
- **Payoffs:** $$ui:∏jSj→Ru_i: \prod_j S_j \to \mathbb{R}.$$
    
- **Mixed strategy:** probability vector $$σi∈Δ(Si)\sigma_i\in \Delta(S_i). A **mixed profile** is σ=(σ1,…,σn)\sigma=(\sigma_1,\dots,\sigma_n).$$
    
- **Expected payoff** under σ\sigma: $$Ui(σ)=Es∼σ[ui(s)]U_i(\sigma)=\mathbb{E}_{s\sim \sigma}[u_i(s)] (multilinear in σ\sigma).$$
    

Mental model: strategies are points on simplices; payoffs are linear in your own mix holding others fixed.

---

## 2) Best responses & dominance

### Best response (BR)

Given opponents’ $$mix σ−i\sigma_{-i},$$ player ii’s **best-response set**:

$$BRi(σ−i)=arg⁡max⁡σi∈Δ(Si)Ui(σi,σ−i).BR_i(\sigma_{-i})=\arg\max_{\sigma_i\in \Delta(S_i)} U_i(\sigma_i,\sigma_{-i}).$$

- In finite games, $$BRi(σ−i)BR_i(\sigma_{-i})$$ is **non-empty**, **convex**, and **upper hemicontinuous** (Week 0 tools).
    

**Operationally:** to find BRs in a 2×2, compare payoffs row-wise/column-wise and mark arrows toward the larger number; equalities mean both actions are best responses.

### Dominance (pure actions)

- **Strict dominance:** action $$aia_i$$ strictly dominates $$bib_i if ui(ai,s−i)>ui(bi,s−i)u_i(a_i,s_{-i})>u_i(b_i,s_{-i}) for **all** s−is_{-i}.$$
    
- **Weak dominance:** $$ui(ai,s−i)≥ui(bi,s−i)u_i(a_i,s_{-i})\ge u_i(b_i,s_{-i})$$ for all $$s−is_{-i}$$ and **strictly** for some.
    
- **Iterated deletion of strictly dominated strategies (IDSDS):** you may remove strictly dominated actions repeatedly; **order doesn’t matter** (safe).  
    Weak dominance deletion can be **order-dependent**; be cautious (can kill equilibria).
    

**Upgrade:** dominance by a **mixed** strategy (an action may be dominated by a lottery over other actions). Strict dominance by mixed strategies is also safely deletable.

**Why this matters:** pruning first simplifies games; remaining actions are **rationalizable** candidates.

---

## 3) Nash equilibrium (NE)

### Definition

A (pure or mixed) profile $$σ⋆\sigma^\star$$ is a **Nash equilibrium** if every player is best-responding:

$$σi⋆∈BRi(σ−i⋆)∀i.\sigma_i^\star \in BR_i(\sigma_{-i}^\star)\quad \forall$$ i.

Equivalently, no unilateral deviation raises payoff.

### Existence (what you use, not full proof)

Every finite game has at least one **mixed** NE (Kakutani on the BR correspondence). Pure NE may or may not exist.

### Pure NE: grid/arrow test (2×2 or small games)

- Draw the payoff matrix.
    
- For each column, mark the row player’s best response(s).  
    For each row, mark the column player’s best response(s).
    
- **Cells with mutual arrows** are pure NE.
    

### Mixed NE (2-player finite): **Indifference principle**

In any mixed NE, each player puts positive probability only on actions that give **equal** expected payoff against the opponent’s mix; actions given zero probability yield **weakly lower** payoff.

For 2×2 games, this becomes a 2-equation system.

---

## 4) The 2×2 mixed-NE workflow (rock-solid and fast)

Label payoffs as:

$$LeftRightTop(a,e)(b,f)Bottom(c,g)(d,h)\begin{array}{c|cc} & \text{Left} & \text{Right}\\\hline \text{Top} & (a,e) & (b,f)\\ \text{Bottom} & (c,g) & (d,h) \end{array}$$

Row payoffs: a,b,c,da,b,c,d. Column payoffs: e,f,g,he,f,g,h.  
Let row mix Top with probability pp, column mix Left with probability qq.

### Step A — Column’s mix q⋆q^\star makes **row** indifferent:

$$aq+b(1−q)⏟Row’s Top=cq+d(1−q)⏟Row’s Bottom  ⇒  q⋆=d−b(a−b)−(c−d)=d−ba−b−c+d.\underbrace{a q + b(1-q)}_{\text{Row’s Top}}=\underbrace{c q + d(1-q)}_{\text{Row’s Bottom}} \;\Rightarrow\; q^\star=\frac{d-b}{(a-b)-(c-d)}=\frac{d-b}{a-b-c+d}.$$

### Step B — Row’s mix p⋆p^\star makes **column** indifferent:

$$ep+g(1−p)=fp+h(1−p)  ⇒  p⋆=h−f(e−f)−(g−h)=h−fe−f−g+h.e p + g(1-p) = f p + h(1-p) \;\Rightarrow\; p^\star=\frac{h-f}{(e-f)-(g-h)}=\frac{h-f}{e-f-g+h}.$$

### Step C — Interior feasibility & supports

- For a **proper** interior mix, both numerators and denominators imply $$p⋆,q⋆∈(0,1)p^\star,q^\star\in(0,1).$$
    
- If $$p⋆p^\star (or q⋆q^\star)$$ falls outside [0,1][0,1], the mixed NE doesn’t use both actions; check pure NE or one-sided support (support enumeration).
    

### Step D — Equilibrium payoffs

Plug $$p⋆,q⋆p^\star,q^\star$$ back into either player’s expected payoff (indifference values).

**Sanity checks**

- If a player strictly prefers one action for **every** qq (or pp), the game has no interior mix for that player; expect pure NE or degenerate mixing.
    
- In **zero-sum** games, the two equilibrium values match (minimax); both players’ supports equalize opponent’s payoffs on support.
    

---

## 5) Classic 2×2 “taxonomy” (pattern recognition)

- **Prisoner’s Dilemma:** one strictly dominant action each; unique inefficient pure NE.
    
- **Stag Hunt (coordination):** two pure NE (safe vs payoff-dominant) + risk dominance concept.
    
- **Battle of the Sexes (anti-coordination with common interest):** two pure NE + one mixed.
    
- **Matching Pennies (zero-sum):** no pure NE; unique mixed.
    

Recognizing which pattern you’re in tells you what to try first (IDSDS vs BR grid vs mixing).

---

## 6) Zero-sum special case (preview)

If $$u1=−u2u_1=-u_2,$$ a 2×2 mixed NE always exists and is unique (unless ties create a continuum). The mix equalizes the opponent’s payoffs on their support (what you just solved). In Week 0 you saw the LP view; here the **indifference equalization** is enough for hand-solving tiny games.

---

## 7) Common pitfalls

- **Using weak dominance indiscriminately.** Can delete valid equilibria; prefer strict dominance first.
    
- **Forgetting support conditions.** In a mixed NE, actions in support must give **exactly** the same payoff; non-support actions must be **no better**.
    
- **Sign mistakes in the 2×2 formulas.** Keep the template handy; verify results sit in [0,1][0,1].
    
- **Assuming uniqueness.** Many general-sum games have multiple NE (compare Stag Hunt vs Battle of the Sexes).
    

---

## 8) Micro-applications to quant

- **Two market makers** setting spreads: each maker’s optimal spread best-responds to the rival’s (inventory/arrival tradeoff) → NE spread profile.
    
- **Ad-auction style choices** (when simplified to 2 slots/2 bidders): anti-coordination structure akin to Battle of the Sexes with private values added later (Week 3–4).
    
- **Spoof detection as a game:** manipulator vs exchange surveillance—zero-sum flavor; the mixed strategy equalization intuition says “randomize inspection” to keep the manipulator indifferent.
    

---

## 9) Practice set (do first; I’ll help step-by-step)

We’ll start with one concrete 2×2 and go **one step at a time**.

**Game A (Stag-Hunt-ish):**

$$LRU(4,4)(0,3)D(3,0)(2,2)\begin{array}{c|cc} & L & R\\\hline U & (4,4) & (0,3)\\ D & (3,0) & (2,2) \end{array}$$

**Q1 (single step):**  
Against **L**, which action(s) are Row’s best response(s), UU or DD?  
(Just name the action(s); we’ll proceed from your answer.)