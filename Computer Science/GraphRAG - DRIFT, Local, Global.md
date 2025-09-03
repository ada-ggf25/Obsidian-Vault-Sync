#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs 

Got you — here’s the clean, no-hand-waving way to tell **Local**, **Global**, and **DRIFT** apart in GraphRAG, plus when to use each.

# The difference in plain English

- **Local** = _entity-centric lookup._  
    Starts from entities the query names (or strongly implies), pulls a small neighborhood from the knowledge graph, and mixes in the most relevant source snippets. Fastest and most precise for “about X/Y/Z” questions.
    
- **Global** = _corpus-level synthesis._  
    Doesn’t start from entities. It reads precomputed **community summaries** (map→reduce) across the whole corpus to answer “what are the main themes/patterns/trends?” Most expensive but best when you want a bird’s-eye view.
    
- **DRIFT** = _hybrid aim + drill-down._  
    First uses **global/community** signals to propose an initial take and targeted follow-up questions, then runs **local** searches for concrete facts, finally fusing everything. Purpose: boost recall/diversity on narrow, under-specified asks without paying full Global cost.
    

# How to pick (decision rule you can apply)

1. **Does the query name specific entities (or you expect the answer near them)?** → **Local.**  
    _Example:_ “Who is Scrooge and what are his relationships?”
    
2. **Is the user asking about the whole collection (themes, top issues, big picture)?** → **Global.**  
    _Example:_ “What are the top themes in this story?”
    
3. **Borderline / under-specified / Local feels brittle?** → **DRIFT.**  
    _Example:_ “Why did outages spike last quarter?” (no entity given). DRIFT lets community summaries “aim” the search, then locally fetches concrete incidents/edges to cite.
    

# What each actually does & cost profile

- **Local**: entity seeding → small k-hop neighborhood + supporting text → answer with citations. **Cost:** low; **Latency:** low.
    
- **Global**: choose a community level → map (per-community partial answers from summaries) → reduce (merge top points) → answer. **Cost:** highest; quality depends on community level chosen.
    
- **DRIFT**: **Primer** (rank top-K community reports; draft + follow-ups) → **Follow-ups** (run Local on those) → **Fusion** (combine/rerank into final). **Cost:** medium—better coverage than Local, cheaper than full Global.
    

---

### Quick cheatsheet

- **Local** → “named things” or clearly scoped facts.
    
- **Global** → “sense-making over the whole corpus.”
    
- **DRIFT** → “I’m not sure where to start; guide me to the right spots, then show receipts.”
    

If you share 2–3 real questions you see, I’ll label each (Local/Global/DRIFT) and suggest concrete parameters (e.g., DRIFT top-K communities, iterations, and token budgets) you can drop into your settings.