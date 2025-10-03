#math #game_theory #quantitative_finance



Great—Week 5 is about **repetition and learning**: how cooperation can be enforced over time, and how simple adaptive algorithms drive play toward equilibrium concepts (CCE/CE). I’ll write everything in Obsidian-friendly LaTeX\LaTeX.

---

# Week 5 — Repeated Games & Learning Dynamics

## 0) Learning goals

By the end you should be able to:

1. Model infinitely repeated games, discounting, and subgame-perfect incentives.
    
2. Use **trigger strategies** and compute discount thresholds for credibility.
    
3. State/apply **Folk Theorems** (feasible &\& enforceable payoffs).
    
4. Understand **no-regret** learning (MWU), **fictitious play**, and links to **(coarse) correlated equilibrium**.
    
5. Recognize **potential**/(**congestion**) games and reason about **Price of Anarchy** bounds.
    

---

## 1) Model: infinitely repeated games with discounting

- **Stage game** $$G=(N,{Si},{ui})G=(N,\{S_i\},\{u_i\})$$ with finite actions $$SiS_i$$ and payoffs $$ui(s)u_i(s).$$
    
- **Horizon:** $$t=1,2,…t=1,2,\dots.$$ **Public histories** $$ht=(s1,…,st−1)h^t=(s^1,\dots,s^{t-1}).$$
    
- **Strategies:** $$σi\sigma_i$$ maps every history $$hth^t$$ to a mixed action on $$SiS_i.$$
    
- **Discounting:** $$δ∈[0,1)\delta\in[0,1).$$ Player ii’s normalized discounted payoff under play path $$(st)t≥1(s^t)_{t\ge1}:$$
    

$$Ui=(1−δ)∑t=1∞δ t−1 ui(st).U_i = (1-\delta)\sum_{t=1}^{\infty} \delta^{\,t-1}\, u_i(s^t).$$

(Equivalent to the undiscounted average payoff when $$δ↑1\delta\uparrow 1.$$)

- **Equilibrium notion:** **Subgame Perfect Equilibrium (SPE)** in the repeated game (every proper subgame is a Nash equilibrium). With perfect monitoring, the **one-deviation principle** applies: no player can profit from a one-shot deviation after any history.
    

---

## 2) Punishments, minmax, and feasibility

- **Minmax (security) payoff** for player ii:
    

$$vimin⁡  =  min⁡s−i∈S−imax⁡si∈Siui(si,s−i).v_i^{\min} \;=\; \min_{s_{-i}\in S_{-i}} \max_{s_i\in S_i} u_i(s_i,s_{-i}).$$

- **Feasible set FF:** the convex hull of stage payoffs $${u(s):s∈S}\{u(s):s\in S\}.$$
    
- **Individually rational (IR) set:** $$FIR={x∈F: xi≥vimin⁡ ∀i}F^{\text{IR}}=\{x\in F:\ x_i\ge v_i^{\min}\ \forall i\}.$$
    

---

## 3) Folk Theorems (perfect monitoring; statements you’ll use)

- **Folk Theorem (Abreu-type, sketch):**  
    Let GG be a finite stage game with perfect monitoring. For any payoff vector $$uˉ\bar u$$ in the **interior** of $$FIRF^{\text{IR}},$$ there exists $$δˉ<1\bar\delta<1$$ such that for all $$δ∈(δˉ,1)\delta\in(\bar\delta,1)$$ there is an **SPE** achieving $$uˉ\bar u.$$
    
- **Interpretation:** If everyone can be credibly punished down to (at least) $$vimin⁡v_i^{\min}$$ and $$\delta$$ is high enough, then any feasible/IR target is implementable.
    

(With public or imperfect monitoring there are refined versions, but Week 5 sticks to perfect monitoring.)

---

## 4) Trigger strategies and δ\delta thresholds

### 4.1 Grim trigger (infinite punishment)

- **Plan:** Play the cooperative profile $$sCs^C$$ forever. If anyone deviates in any period, play a punishment path forever (e.g., a profile that delivers $$vimin⁡v_i^{\min}$$ to the deviator).
    

Let

$$uiC:=ui(sC),uiD:=max⁡siui(si,s−iC)$$(one-shot best deviation payoff),$$viP:=vimin⁡.u_i^C := u_i(s^C),$$$$ \quad u_i^D := \max_{s_i} u_i(s_i, s^C_{-i}) \quad (\text{one-shot best deviation payoff}), \quad v_i^{P} := v_i^{\min}.$$

- **Cooperate forever value:**
    

$$ViC  =  uiC1−δ.V_i^C \;=\; \frac{u_i^C}{1-\delta}.$$

- **Deviation value (one step then punishment forever):**
    

$$ViD  =  uiD  +  δ viP1−δ.V_i^D \;=\; u_i^D \;+\; \frac{\delta\, v_i^{P}}{1-\delta}.$$

- **Incentive constraint (IC):** ViC≥ViDV_i^C \ge V_i^D iff
    

 $$uiC  ≥  (1−δ) uiD  +  δ viP ⟺ δ  ≥  uiD−uiC uiD−viP .\boxed{\,u_i^C \;\ge\; (1-\delta)\,u_i^D \;+\; \delta\, v_i^{P}\,} \quad\Longleftrightarrow\quad \boxed{\,\delta \;\ge\; \frac{u_i^D - u_i^C}{\,u_i^D - v_i^{P}\,}.}$$

### 4.2 Finite LL-period punishments

Punish with payoff $$uiPu_i^P$$ for L periods, then restore cooperation.

$$ViD  =  uiD  +  δ∑k=0L−1δkuiP  +  δL+1uiC1−δ  =  uiD  +  δ(1−δL)1−δuiP  +  δL+11−δuiC.V_i^D \;=\; u_i^D \;+\; \delta \sum_{k=0}^{L-1}\delta^{k} u_i^P \;+\; \delta^{L+1}\frac{u_i^C}{1-\delta} \;=\; u_i^D \;+\; \frac{\delta(1-\delta^L)}{1-\delta}u_i^P \;+\; \frac{\delta^{L+1}}{1-\delta}u_i^C.$$

$$IC: uiC1−δ≥ViD\frac{u_i^C}{1-\delta} \ge V_i^D.$$ Solve for $$\delta$$ to get a (usually milder) threshold as a function of L.

### 4.3 Prisoner’s Dilemma (canonical numbers)

Let stage payoffs satisfy $$T>R>P>ST>R>P>S$$ (Temptation, Reward, Punishment, Sucker). With grim:

$$VC=R1−δ,VD=T+δP1−δ.V_C=\frac{R}{1-\delta}, \qquad V_D=T+\frac{\delta P}{1-\delta}.$$

$$IC VC≥VDV_C\ge V_D$$ gives

 $$δ  ≥  T−R T−P .\boxed{\,\delta \;\ge\; \frac{T-R}{\,T-P\,}.}$$

(You can similarly compute thresholds for finite LL punishments.)

> **Note:** Strategies like **Tit-for-Tat** can support cooperation in some environments but are **not** generally SPE-robust (especially under trembles/noise). Grim or calibrated finite punishments make ODP checks clean.

---

## 5) Learning in games

### 5.1 External regret and Multiplicative Weights (MWU)

- **External regret** after TT rounds for player i (actions $$aita_i^t, opponents a−ita_{-i}^t):$$
    

$$RText  =  max⁡a∈Si∑t=1Tui(a,a−it)  −  ∑t=1Tui(ait,a−it).R_T^{\text{ext}} \;=\; \max_{a\in S_i} \sum_{t=1}^T u_i(a, a_{-i}^t) \;-\; \sum_{t=1}^T u_i(a_i^t, a_{-i}^t).$$

No-regret means $$RText/T→0R_T^{\text{ext}}/T \to 0.$$

- **MWU (loss form)** for $$K=∣Si∣K=|S_i|$$ actions with losses $$ℓt(a)∈[0,1]\ell_t(a)\in[0,1]:$$  
    Initialize $$w1(a)=1w_1(a)=1. For t=1,…,Tt=1,\dots,T,$$
    

$$pt(a)=wt(a)∑bwt(b),wt+1(a)=wt(a) exp⁡(−η ℓt(a)).p_t(a)=\frac{w_t(a)}{\sum_b w_t(b)},\qquad w_{t+1}(a)=w_t(a)\,\exp(-\eta\,\ell_t(a)).$$

Guarantee (choose $$η≍ln⁡KT\eta \asymp \sqrt{\tfrac{\ln K}{T}}):$$

 $$RText  =  O ⁣(Tln⁡K) .\boxed{\,R_T^{\text{ext}} \;=\; O\!\left(\sqrt{T\ln K}\right)\,}.$$

- **Key equilibrium connection:** If **every** player uses a no-external-regret algorithm, the **empirical distribution** of play converges to the set of **Coarse Correlated Equilibria (CCE)**.
    

**CCE definition.** A distribution $$π\pi$$ over action profiles $$s∈Ss\in S$$ is a CCE if, for each player ii and any fixed deviation $$ai′a_i',$$

$$Es∼π[ui(s)]  ≥  Es∼π[ui(ai′,s−i)].\mathbb{E}_{s\sim\pi}\big[u_i(s)\big] \;\ge\; \mathbb{E}_{s\sim\pi}\big[u_i(a_i', s_{-i})\big].$$

- **Upgrade:** If players drive **swap regret** to zero, empirical play approaches the set of **Correlated Equilibria (CE)**.
    

**CE definition.** With signal/recommendation aia_i drawn from π\pi,

$$E ⁣[ui(ai,a−i)∣ai]  ≥  E ⁣[ui(ai′,a−i)∣ai]∀i, ∀ai′.\mathbb{E}\!\left[u_i(a_i,a_{-i})\mid a_i\right] \;\ge\; \mathbb{E}\!\left[u_i(a_i',a_{-i})\mid a_i\right] \quad\forall i,\ \forall a_i'.$$

### 5.2 Fictitious play (FP)

- Each period, best-respond to opponents’ **empirical frequencies**.
    
- **Converges** to minimax in two-player zero-sum; converges to a (possibly mixed) NE in potential games; can **cycle** in general (e.g., Shapley’s game).
    

---

## 6) Potential and congestion games

### 6.1 Exact potential games

A finite game is an **exact potential game** if there exists $$Φ:S→R\Phi:S\to\mathbb{R}$$ such that for all ii, all ss, and all $$si′s_i',$$

$$
ui(si′,s−i)−ui(si,s−i)  =  Φ(si′,s−i)−Φ(si,s−i).u_i(s_i',s_{-i}) - u_i(s_i,s_{-i}) \;=\; \Phi(s_i',s_{-i}) - \Phi(s_i,s_{-i}).
$$

**Implications:** Pure-strategy NE exist; any **better-reply** path strictly increases Φ and thus **converges** (finite state space).

### 6.2 Congestion games (atomic)

- Players choose subsets of resources EE.
    
- Each resource e has latency/cost $$ce(x)c_e(x)$$ depending on the number of users xx.
    
- Player ii’s cost: $$Ci(s)=∑e∈sice(xe(s))C_i(s)=\sum_{e\in s_i} c_e(x_e(s)).$$
    
- **Rosenthal potential:**
    

$$Φ(s)  =  ∑e∈E ∑k=1xe(s)ce(k),\Phi(s) \;=\; \sum_{e\in E}\ \sum_{k=1}^{x_e(s)} c_e(k),$$

so congestion games are potential games ⇒ **pure NE exist** and local improvement dynamics converge.

---

## 7) Price of Anarchy (PoA) and stability

Let W(s)W(s) be social welfare (to maximize) or $$C(s)$$ social cost (to minimize).

- **Welfare version:**
    

$$PoA  =  min⁡s∈NEW(s)max⁡sW(s)∈(0,1].\mathrm{PoA} \;=\; \frac{\min_{s\in \text{NE}} W(s)}{\max_{s} W(s)} \in (0,1].$$

- **Cost version:**
    

$$PoA  =  max⁡s∈NEC(s)min⁡sC(s)≥1.\mathrm{PoA} \;=\; \frac{\max_{s\in \text{NE}} C(s)}{\min_{s} C(s)} \ge 1.$$

- **Price of Stability (PoS):** best equilibrium vs optimum (replace max/min over NE accordingly).
    

**Classical bounds to know:**

- **Nonatomic selfish routing** with **affine** latencies $$ℓe(x)=aex+be\ell_e(x)=a_ex+b_e: PoA≤43\mathrm{PoA}\le \tfrac{4}{3}.$$
    
- **Atomic congestion** with affine latencies: $$PoA\mathrm{PoA}$$ for pure NE is at most $$52 \tfrac{5}{2}$$ (tight for certain instances).
    

(These are standard reference points; details depend on the exact model and tie-breaking.)

**Pigou network intuition (nonatomic):** Equilibrium overuses the congestible edge; the 43\tfrac{4}{3} arises from comparing equilibrium vs socially optimal flow split.

---

## 8) Quant/micro links (why you care)

- **Inventory-anchored cooperation:** Repeated maker–taker interactions can sustain tighter spreads/priority rules when δ\delta is high and punishments are credible (e.g., routing away order flow).
    
- **Adaptive execution:** MWU-style updates under adversarial slippage produce **no-regret** policies; time-averaged play approximates **CCE** against an adaptive market.
    
- **Market fragmentation = PoA:** Decentralized venue selection can be inefficient (latency/congestion externalities); PoA frames the gap between equilibrium and coordinated optimum.
    
- **Potential-like objectives:** Many multi-agent RL market simulations exploit potential functions for convergence of best-response learning.
    

---

## 9) Worked mini-templates

### 9.1 General grim-trigger IC

Target $$sCs^C$$ with payoffs $$uiCu_i^C;$$ one-shot deviation $$uiDu_i^D;$$ punishment payoff $$viPv_i^P.$$

$$IC:uiC1−δ  ≥  uiD+δviP1−δ⟺ δ≥uiD−uiCuiD−viP .\text{IC:}\quad \frac{u_i^C}{1-\delta} \;\ge\; u_i^D + \frac{\delta v_i^P}{1-\delta} \quad\Longleftrightarrow\quad \boxed{\,\delta \ge \frac{u_i^D - u_i^C}{u_i^D - v_i^P}\,}.$$

### 9.2 Finite-LL punishment IC (plug-and-play)

$$uiC1−δ  ≥  uiD+δ(1−δL)1−δuiP+δL+11−δuiC.\frac{u_i^C}{1-\delta} \;\ge\; u_i^D + \frac{\delta(1-\delta^L)}{1-\delta}u_i^P + \frac{\delta^{L+1}}{1-\delta}u_i^C.$$

Rearrange to solve for $$\delta$$ given $$(uiC,uiD,uiP,L)(u_i^C,u_i^D,u_i^P,L).$$

### 9.3 MWU regret bound (sketch form)

For $$η=2ln⁡KT\eta=\sqrt{\tfrac{2\ln K}{T}}$$ and losses in [0,1][0,1],

$$RText  ≤  2Tln⁡K.R_T^{\text{ext}} \;\le\; \sqrt{2T\ln K}.$$

---

## 10) Optional coding (short, practical)

- **MWU demo:** Simulate a $$2×22\times2$$ zero-sum game; plot external regret $$∼O(T)\sim O(\sqrt{T}),$$ and the empirical distribution approaching the value mixed strategy.
    
- **Repeated PD:** Implement grim with noise ϵ\epsilon; estimate cooperation rate vs $$\delta$$ and compare $$finite-LL$$ punishments.
    
- **Pigou network (nonatomic):** Compute equilibrium flow vs optimal flow; verify the $$43\tfrac{4}{3}$$ gap for affine latencies.
    

---

## 11) Common pitfalls

- Forgetting **one-deviation** checks after off-path histories (SPE requires them).
    
- Using punishments below $$vimin⁡v_i^{\min}$$ (not credible).
    
- Mixing **CCE** and **CE** constraints; remember: no-external-regret ⇒ CCE, no-swap-regret ⇒\Rightarrow CE.
    
- Assuming fictitious play always converges (it doesn’t, outside special classes).
    

---

### Quick check (one step)

Take a stage profile with $$uiC=3u_i^C=3, uiD=5u_i^D=5, and viP=1v_i^{P}=1.$$ Using grim trigger, what is the **minimum** δ that satisfies the IC? (Just give the fraction.)

