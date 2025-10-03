#math #game_theory #quantitative_finance 

Here’s a compact “core concepts” cheat-sheet you can drop into your notes. I grouped ideas by what you need to define, how games are represented, and the main solution concepts (plus a few must-know special classes and tools).

# Core objects

- **Players, strategies, payoffs (utility):** A game specifies who moves, what they can do, and their payoffs; preferences are represented by utilities. Strategic (normal) form lists actions and payoffs; extensive form uses a game tree with **information sets** to capture what players know when they move. ([gametheory101.com](https://gametheory101.com/courses/game-theory-101/bayesian-nash-equilibrium/?utm_source=chatgpt.com "Bayesian Nash Equilibrium - Game Theory 101"))
    
- **Mixed & behavioral strategies:** Mixed = probability over pure actions (normal form). Behavioral = probability over actions at each information set (extensive form); with perfect recall they’re equivalent. ([gametheory101.com](https://gametheory101.com/courses/game-theory-101/bayesian-nash-equilibrium/?utm_source=chatgpt.com "Bayesian Nash Equilibrium - Game Theory 101"))
    
- **Knowledge & beliefs:** **Common knowledge** (everyone knows, knows that everyone knows, ad infinitum) underpins many predictions; **Bayesian games** model **incomplete information** via types and priors (Harsanyi transformation). ([Stanford Encyclopedia of Philosophy](https://plato.stanford.edu/archives/fall2005/entries/common-knowledge/?utm_source=chatgpt.com "Common Knowledge"))
    

# How we represent games

- **Strategic (normal) form** vs **extensive form** (trees, info sets, chance moves). Convert incomplete-info games to extensive form by letting Nature draw types. ([gametheory101.com](https://gametheory101.com/courses/game-theory-101/bayesian-nash-equilibrium/?utm_source=chatgpt.com "Bayesian Nash Equilibrium - Game Theory 101"))
    
- **Repeated games:** Stage game played over time; key results include **Folk theorems** (with discounting, many feasible individually rational payoffs can be sustained in equilibrium when players are patient). ([MIT OpenCourseWare](https://ocw.mit.edu/courses/14-12-economic-applications-of-game-theory-fall-2012/54f153ef7ed7800f367ce2b4021a18db_MIT14_12F12_chapter12.pdf?utm_source=chatgpt.com "Chapter 12 Repeated Games"))
    

# Baseline solution concepts

- **Dominance & iterated deletion:** Eliminate **strictly dominated** strategies; what survives can predict play under common knowledge of rationality. ([homepages.cwi.nl](https://homepages.cwi.nl/~apt/stra/ch3.pdf?utm_source=chatgpt.com "Chapter 3 Strict Dominance"))
    
- **Best responses & Nash equilibrium (NE):** A profile where each strategy is a best response to the others; existence in finite games via fixed-point theorems (Kakutani/Brouwer). ([Computer Science Department](https://www.cs.upc.edu/~ia/nash51.pdf?utm_source=chatgpt.com "Non Cooperative Games John Nash"))
    
- **Zero-sum & minimax:** In two-player zero-sum, value equals max–min = min–max; mixed strategies suffice (**von Neumann’s minimax theorem**). ([Wikipedia](https://en.wikipedia.org/wiki/Minimax_theorem?utm_source=chatgpt.com "Minimax theorem"))
    

# Refinements for dynamics and information

- **Subgame-perfect equilibrium (SPE):** NE that induces an NE in every subgame (rules out non-credible threats). In repeated games, check with the **one-shot deviation principle**. ([MIT OpenCourseWare](https://ocw.mit.edu/courses/17-810-game-theory-spring-2021/mit17_810s21_lec5.pdf?utm_source=chatgpt.com "17.810S21 Game Theory, Lecture Slides 5: Repeated Games"))
    
- **Trembling-hand perfect equilibrium (Selten):** NE robust to tiny mistakes. ([Wikipedia](https://en.wikipedia.org/wiki/Trembling_hand_perfect_equilibrium?utm_source=chatgpt.com "Trembling hand perfect equilibrium"))
    
- **Sequential / Perfect Bayesian equilibrium (PBE):** For dynamic games with (possibly) incomplete info; combine strategies with **beliefs** consistent with Bayes’ rule and sequential rationality (sequential equilibrium gives a rigorous consistency notion). ([Wikipedia](https://en.wikipedia.org/wiki/Sequential_equilibrium?utm_source=chatgpt.com "Sequential equilibrium"))
    

# Incomplete information

- **Bayesian Nash equilibrium (BNE):** Each type plays a best response to beliefs about others’ types (from a common prior). Use Harsanyi types; Nature moves first. ([Stanford University](https://web.stanford.edu/~jdlevin/Econ%20203/Bayesian.pdf?utm_source=chatgpt.com "Games of Incomplete Information"))
    

# Beyond independent play

- **Correlated equilibrium (Aumann):** A mediator privately recommends actions; following the recommendation is optimal given others do. Set of CEs is convex, often larger than NE; learning/no-regret dynamics converge to (coarse) CE. ([Wikipedia](https://en.wikipedia.org/wiki/Correlated_equilibrium?utm_source=chatgpt.com "Correlated equilibrium"))
    

# Cooperative ideas (brief)

- **Core:** Allocations no coalition can block (no subset can make everyone in it strictly better given what they can achieve alone). ([Wikipedia](https://en.wikipedia.org/wiki/Core_%28game_theory%29?utm_source=chatgpt.com "Core (game theory)"))
    
- **Shapley value:** Unique way to split surplus by average **marginal contributions** over all coalition orders (satisfies efficiency, symmetry, additivity, dummy). ([Wikipedia](https://en.wikipedia.org/wiki/Shapley_value "Shapley value - Wikipedia"))
    
- **Nash bargaining solution:** Axiomatic solution maximizing the **Nash product** subject to Pareto efficiency, symmetry, scale invariance, IIA. ([MIT OpenCourseWare](https://ocw.mit.edu/courses/6-254-game-theory-with-engineering-applications-spring-2010/837a6dbd4edfe215c710dd0cf0649021_MIT6_254S10_lec14.pdf?utm_source=chatgpt.com "Nash bargaining solution"))
    

# Special game classes & selection

- **Potential games (Monderer–Shapley):** Players’ incentives align with a single **potential function**; best-response dynamics converge to pure NE. ([qwone.com](https://qwone.com/~jason/trg/papers/monderer-potential-96.pdf?utm_source=chatgpt.com "Potential Games"))
    
- **Supermodular games / strategic complementarities:** Increasing differences imply monotone best responses and existence of (extreme) pure NE. ([MIT OpenCourseWare](https://ocw.mit.edu/courses/6-254-game-theory-with-engineering-applications-spring-2010/522279c2bc2c195a7fd395eb60eff69e_MIT6_254S10_lec07.pdf?utm_source=chatgpt.com "Supermodular games"))
    
- **Equilibrium selection:** **Payoff dominance** vs **risk dominance** (Harsanyi–Selten); the latter often predicts which equilibrium emerges under uncertainty. ([Wikipedia](https://en.wikipedia.org/wiki/Risk_dominance?utm_source=chatgpt.com "Risk dominance"))
    

# Learning & evolution

- **Fictitious play:** Best-respond to empirical frequencies (converges in some classes). ([Proceedings of Machine Learning Research](https://proceedings.mlr.press/v37/heinrich15.pdf?utm_source=chatgpt.com "Fictitious Self-Play in Extensive-Form Games"))
    
- **No-regret learning:** Average play approaches (coarse) correlated equilibrium. ([theory.stanford.edu](https://theory.stanford.edu/~tim/f13/l/l17.pdf?utm_source=chatgpt.com "Algorithmic Game Theory Lecture #17: No-Regret Dynamics"))
    
- **Evolutionary game theory:** **ESS** (evolutionarily stable strategy) and **replicator dynamics** describe population adjustments; ESS resists invasion by mutants. ([Allesina λab](https://stefanoallesina.github.io/Theoretical_Community_Ecology/game-theory-and-replicator-dynamics.html?utm_source=chatgpt.com "Lecture 6 Game theory and replicator dynamics"))
    

---

If you want this turned into a one-page LaTeX/Obsidian-friendly sheet with a few canonical formulas (NE, minimax, Shapley, Nash product) and a tiny glossary, say the word and I’ll format it.