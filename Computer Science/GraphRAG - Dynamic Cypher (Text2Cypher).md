#graphs #computer_science #AI #machine_learning #LLM  #data_science #GenAI #database

[[GraphRAG - Agentic Traversal Vs Dynamic Cypher (Text2Cypher)]]

**Dynamic Cypher (Text2Cypher)**  
Let an LLM emit Cypher/SPARQL from the natural-language question + schema hint.  
_Use when:_ you want flexibility over a curated KG without writing every template.  
_Watch for:_ query injection / dangerous ops—sandbox + allow-list + cost caps.

---

# What each does

## Dynamic Cypher / Text2Cypher

- **How it works:**
    
    1. LLM reads your question + a sanitized graph schema.
        
    2. Generates a **single Cypher** query (sometimes parameterised).
        
    3. Runs it and returns rows/records (optionally post-processed).
        
- **Best for:** precise, schema-aligned asks: “Who owns Service X?”, “Incidents involving Service X in the last 7 days”, “Shortest path between A and B”.
    
- **Pros:** low latency; deterministic/explainable; easy to audit; great with governed KGs.
    
- **Cons / gotchas:** query-hallucination or injection risks; brittle if schema context is incomplete; not great when the question is fuzzy or needs multi-hop evidence gathering across different modalities.
    

**Typical guardrails:** read-only role, allow-listed clauses, strict schema prompt, parameterised queries, time/row limits, and a validator that rejects unsafe Cypher.