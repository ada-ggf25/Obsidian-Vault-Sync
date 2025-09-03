#graphs #computer_science #AI #machine_learning #LLM  #data_science #GenAI #database

[[GraphRAG - Agentic Traversal Vs Dynamic Cypher (Text2Cypher)]]

**Agentic Traversal**  
Treat each retriever as a tool. The LLM plans → runs a retriever → inspects results → decides next step (iterate) until it has enough context.  
_Use when:_ questions are heterogeneous and require multi-step fetching.  
_Watch for:_ latency and tool-use loops—set step/time budgets.

---

## Agentic Traversal

- **How it works:**  
    The LLM acts as a **controller**: it plans → calls a retriever → inspects results → decides the next step. Tools might include:
    
    - vector/full-text seed search,
        
    - neighborhood or path traversals,
        
    - **Text2Cypher**,
        
    - query templates,
        
    - global/community summaries.  
        It loops until a stop condition (enough evidence, step/latency budget).
        
- **Best for:** ambiguous or **multi-step** questions: “Why did Service A’s latency spike last week?”, “How is vendor B implicated in outages affecting region EU-West?”.
    
- **Pros:** flexible; can combine multiple evidence types; better recall on messy asks.
    
- **Cons / gotchas:** higher latency/cost; non-deterministic plans; needs budgets (max steps/tools), and solid tool selection/reranking to avoid loops.
    

**Common patterns:** ReAct-style “think-act-observe”, tool selection with function-calling, per-step re-ranking, caching, and final answer synthesis with citations.