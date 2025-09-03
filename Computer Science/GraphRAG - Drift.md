#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs 

[[GraphRAG - DRIFT, Local, Global]]

**DRIFT** is a third query mode in Microsoft’s GraphRAG—alongside **Local** and **Global**—that blends the two: it uses **global/community summaries to guide a local search** so you get broad coverage _and_ specific, citeable facts. Officially, DRIFT stands for **Dynamic Reasoning and Inference with Flexible Traversal**.

### What DRIFT does (in practice)

1. **Primer (global cueing):** Embed the user query (often with HyDE expansion), score it against **community reports**, and take the top-K communities. From these, produce an initial answer **and** a set of targeted follow-up questions.
    
2. **Follow-ups (local refinement):** Run **local search** for each follow-up to pull concrete entities, relationships, and supporting text; iterate for a small, fixed number of rounds.
    
3. **Output hierarchy (fusion):** Combine and rank the intermediate answers into a final, grounded response (with the usual GraphRAG citations).
    

This design **widens the starting set** for local queries (because it’s seeded by community knowledge), which tends to surface more relevant facts than plain Local. In Microsoft’s benchmark over AP News articles, DRIFT beat Local on **comprehensiveness (78% of questions)** and **diversity (81%)** by LLM-judge evaluation.

### When to use DRIFT vs. Local vs. Global

- **Local:** you already name entities or expect the answer in a few nearby chunks. Fastest.
    
- **Global:** you want a corpus-wide synthesis (themes across communities). Most costly.
    
- **DRIFT:** your question is _narrow but under-specified_, or Local feels brittle; DRIFT lets global structure “aim” your local hops, often improving recall without paying full Global cost.
    

### How it’s implemented (at a glance)

- Exposed as the **DRIFT Search** mode in the GraphRAG docs/code; you configure the LLM, context builder, hyperparameters, token budget tracking, and query state for the follow-up loop.
    
- Pattern-wise, you can think of it as: **community selection → question decomposition → local retrieval → re-rank & package**—a multi-stage pipeline many pattern catalogs now reference.
    

If you want, tell me your typical queries and I’ll suggest whether to route them to Local, Global, or DRIFT—and with what DRIFT parameters (K for communities, iterations, and token budget).