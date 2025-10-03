#math #game_theory #quantitative_finance

Love it—Week 2 is where games get _dynamic_. You’ll learn to read game trees, enforce credibility with **subgame perfection**, and handle information with **beliefs** (PBE). I’ll keep it tight but rigorous, and we’ll do one small check-in question at the end.

---

# Week 2 — Extensive Form, Credibility, and Signaling

## 1) Objects: game trees, info, strategies

- **Histories / nodes.** A finite rooted tree; terminal nodes zz have payoffs $$ui(z)u_i(z).$$
    
- **Player function.** $$ι(h)\iota(h)$$ says who moves after history hh; **chance** moves allowed with probabilities.
    
- **Information sets (infosets).** A partition of decision nodes where the player can’t tell which node they’re at; all nodes in an infoset share the same available actions.
    
    - **Perfect information:** all infosets are singletons.
        
    - **Perfect recall:** no player forgets past own moves or info—critical for results below.
        
- **(Pure) strategy for player ii.** A complete plan: one action at **every** infoset of ii.
    
- **Behavioral strategy.** At each infoset HH, a distribution over actions.  
    **Kuhn’s Theorem (perfect recall):** mixed strategies ↔\leftrightarrow behavioral strategies are outcome-equivalent. So in trees with perfect recall, it’s safe to think “randomize at each infoset.”
    

**Mental model:** A strategy isn’t “what I do on the path”—it’s a full contingency plan for **every** node you could reach.

---

## 2) Subgames & credibility

- A **subgame** starts at a node whose entire infoset is contained in that node (so you aren’t “cutting” an infoset) and contains all its descendants.
    
- **Subgame Perfect Nash Equilibrium (SPNE).** A strategy profile that induces a Nash equilibrium in **every** subgame (including the whole game).
    
    - Eliminates **non-credible threats**: if you wouldn’t actually carry out a threatened action when the subgame arrives, SPNE won’t allow it.
        

**Entry deterrence cliché:** Incumbent threatens a price war if entry occurs. If a price war is _not_ optimal once entry happens, SPNE removes that empty threat; the entrant anticipates the _actual_ best response.

---

## 3) Backward induction (BI): algorithm & properties

For finite **perfect-information** games with no payoff ties:

1. **Start at leaves.** At the last mover’s decision, pick their best action (replace the subgame with its equilibrium payoff).
    
2. **Roll up one layer** and repeat until the root.
    
3. The resulting strategy profile is **the unique SPNE** (generic payoffs).
    

With ties: BI may yield multiple SPNE (choose any best action at a tie; all completions that are best in each subgame are SPNE).

**Why BI works:** Each step solves a subgame; by induction, you get a Nash equilibrium of every subgame → SPNE.

---

## 4) One-Deviation Principle (ODP)

In any **finite extensive game with perfect recall**, a strategy profile is sequentially optimal for a player **iff** at every infoset of that player, there is no **profitable one-shot deviation** (change the next move at that infoset, then resume the original plan).

- Super useful to verify SPNE in trees where simultaneous BI isn’t convenient (e.g., multiple infosets per player, chance moves).
    
- Intuition: because you can re-optimize later, you only need to check “local” one-step deviations.
    

---

## 5) Sequential rationality & beliefs: PBE (the workhorse)

When there’s **imperfect information**, you need beliefs over where you are in the tree.

**Perfect Bayesian Equilibrium (PBE)** consists of:

- **Strategies** for all players (behavioral is fine).
    
- **Beliefs** at every infoset about which node of that infoset has been reached.
    
- **Sequential rationality:** given beliefs, each player’s strategy maximizes expected payoff at every infoset.
    
- **Bayes updating on-path:** beliefs arise from prior + strategies + Bayes’ rule wherever possible (at infosets reached with positive probability).
    

(“Off-path” beliefs aren’t pinned down by Bayes; different reasonable choices can create multiple PBEs. Refinements like **sequential equilibrium** or **the intuitive criterion** restrict those, but PBE is enough for Week 2.)

---

## 6) Signaling games (dynamic incomplete info)

Basic **sender–receiver** template (Spence/Kyle-like intuitions):

- Nature draws Sender’s type $$θ\theta$$ (private). Sender picks a **signal** mm (may have cost). Receiver observes mm, chooses action aa.
    
- Payoffs $$uS(θ,m,a)u_S(\theta,m,a), uR(θ,m,a)u_R(\theta,m,a).$$
    

**Equilibrium flavors:**

- **Separating:** different types choose different messages; Receiver infers type exactly; beliefs put prob. 1 on that type after each message; IC constraints ensure no type wants to mimic another.
    
- **Pooling:** all types send the same message; Receiver’s belief equals prior; incentives ensure no type profits by deviating given how Receiver would respond to off-path messages.
    
- **Hybrid/partial separation:** some types pool, others separate.
    

**How to solve quickly (playbook):**

1. **Pick a candidate pattern** (separating or pooling).
    
2. **Receiver best-response** given beliefs induced by the pattern.
    
3. **Sender IC/IR:** each type prefers its prescribed message given Receiver’s response.
    
4. **Beliefs:** on-path = Bayes; **off-path** beliefs chosen to support the candidate if possible.
    
5. If any step fails, try another pattern. (We revisit auctions Week 4; this template is the same.)
    

---

## 7) Cheap talk (costless messages)

Messages have **no direct cost** and don’t change payoffs except via Receiver’s action.

- **Babbling equilibrium** always exists: Receiver ignores messages; Sender’s strategy arbitrary.
    
- With partially aligned preferences (e.g., quadratic loss with bias bb), **Crawford–Sobel**: equilibria partition the type space into intervals; messages reveal coarse bins (“I’m low/medium/high”), not full type. More alignment → more bins (up to full revelation); more bias → coarser or only babbling.
    

Quant lens: think analysts’ notes or chatroom talk—informative if incentives align enough; otherwise, markets discount it.

---

## 8) Classic examples to internalize

- **Ultimatum game (perfect info):** BI → proposer offers minimal positive amount; responder accepts (SPNE). Behavioral deviations/experiments differ—useful for mechanism realism.
    
- **Stackelberg quantity competition:** Leader commits; BI gives follower’s BR, leader anticipates it (commitment advantage).
    
- **Entry deterrence:** SPNE removes empty threats; adding sunk capacity can restore credibility (changes subgame payoffs).
    
- **Signaling for quality:** High type may burn resources (costly signal) to separate; low type won’t imitate if cost too high.
    
- **Cheap talk with bias:** With small bias, Receiver partially learns; with large bias, babbling.
    

---

## 9) Common pitfalls

- **Illegal subgames.** You can’t start a subgame by slicing through an infoset.
    
- **Confusing NE with SPNE.** NE in the _strategic form_ can include non-credible threats; SPNE fixes that.
    
- **Forgetting perfect recall** when invoking Kuhn/ODP.
    
- **Off-path beliefs hand-waving.** In PBE, you must specify them (even if just “put prob. 1 on the type that makes deviation worst for the deviator,” when consistent).
    

---

## 10) Mini “how-to” checklists

### Backward induction (finite perfect info)

1. Identify terminal payoffs.
    
2. For last mover at each leaf-parent, select best action; collapse.
    
3. Repeat to root; read off strategies.
    
4. If ties, note the set of SPNE (any tie-consistent completion).
    

### Verifying SPNE via ODP (imperfect info with perfect recall)

1. Propose a full strategy profile.
    
2. At each player’s infoset, check _one-shot_ deviations (holding continuation fixed).
    
3. If none profitable and beliefs are consistent (if needed), you’re done.
    

### Solving a simple signaling PBE

1. Guess pooling or separating.
    
2. Compute Receiver BR and on-path beliefs.
    
3. Check Sender IC.
    
4. Choose off-path beliefs to sustain (if possible).
    
5. If contradictions, change the guess.
    

---

## 11) Quant connections (why you care)

- **Credible commitment** (Stackelberg) ≈ pre-trade quotes, inventory constraints: move first to shape the follower’s BR.
    
- **Signaling** ≈ order flow from informed traders; market makers infer types and set prices $$=E[v∣flow]=\mathbb{E}[v \mid \text{flow}]$$ (Bayes on path).
    
- **Cheap talk** ≈ research notes, forum chatter: informative only with sufficient alignment; otherwise revert to priors (babbling).
    

---

## 12) Optional coding ideas (1–2 h total)

- **Backward-induction solver:** represent a tree (dict of nodes → children/payoffs). Compute SPNE by collapsing from leaves.
    
- **ODP checker:** given a candidate profile and beliefs, iterate infosets and test one-shot deviations.
    
- **Toy signaling simulator:** two types, binary messages/actions. Enumerate pooling/separating candidates, check IC/BR/beliefs; print all PBEs.
    

---

## Quick comprehension check (one step)

Consider the **ultimatum game** with $10: Proposer offers $$$x∈{0,…,10}x\in\{0,\dots,10\};$$ Responder accepts (A) or rejects (R). If A, payoffs $$(10−x, x)(10-x,\ x);$$ if $$R, (0,0)(0,0).$$  
**Question:** Starting from the last move, what is the Responder’s best response to an offer $$x>0x>0?$$ (Just “accept” or “reject,” and why in one line.)  
After your answer, we’ll finish the backward-induction logic together.