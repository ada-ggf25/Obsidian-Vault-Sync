#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs

[[GraphRAG - Agentic Traversal]]
[[GraphRAG - Dynamic Cypher (Text2Cypher)]]
## Summary

GraphRAG refers to a family of Retrieval-Augmented Generation (RAG) techniques that leverage graph-structured data to improve the factuality, explainability, and reasoning capabilities of large language models. There are at least **five** distinct GraphRAG variants in the literature: **Standard Knowledge GraphRAG**, **Medical GraphRAG**, **Graph Foundation Model RAG (GFM-RAG)**, **HyperGraphRAG**, and **Temporal GraphRAG (T-GRAG)**. They differ primarily in the structure of the graphs they use (e.g., simple knowledge graphs vs. hypergraphs), the retrieval and reasoning mechanisms employed (e.g., graph neural networks vs. template-based queries), and their target domains (e.g., general QA vs. medical). Use cases range from open-domain question answering and multi-hop reasoning to safety-critical medical advice and temporal query resolution.

---

## Types of GraphRAG

### 1. Standard Knowledge GraphRAG

Standard GraphRAG augments RAG with a traditional knowledge graph, transforming documents or text corpora into nodes and edges, then retrieving subgraphs for context-rich generation ([Graph Database & Analytics](https://neo4j.com/blog/genai/what-is-graphrag/?utm_source=chatgpt.com "What Is GraphRAG?")) ([Wikipédia](https://en.wikipedia.org/wiki/Retrieval-augmented_generation?utm_source=chatgpt.com "Retrieval-augmented generation")).

### 2. Medical GraphRAG (MedGraphRAG)

MedGraphRAG tailors the GraphRAG paradigm to the medical domain by constructing triple-linked graphs that connect user documents with controlled vocabularies and credible medical sources; retrieval combines top-down precise search with bottom-up refinement for evidence-based responses ([arXiv](https://arxiv.org/abs/2408.04187?utm_source=chatgpt.com "Medical Graph RAG: Towards Safe Medical Large Language Model via Graph Retrieval-Augmented Generation")).

### 3. Graph Foundation Model RAG (GFM-RAG)

GFM-RAG introduces a Graph Foundation Model—a graph neural network pretrained on millions of triples—that reasons over unseen graphs without fine-tuning, enabling generalizable, efficient retrieval across diverse datasets ([arXiv](https://arxiv.org/abs/2502.01113?utm_source=chatgpt.com "GFM-RAG: Graph Foundation Model for Retrieval Augmented Generation")).

### 4. HyperGraphRAG

HyperGraphRAG extends GraphRAG by representing n-ary relations as hyperedges rather than binary edges, capturing complex multi-entity facts; it includes a hypergraph construction, retrieval strategy, and hypergraph-guided generation pipeline ([arXiv](https://arxiv.org/abs/2503.21322?utm_source=chatgpt.com "HyperGraphRAG: Retrieval-Augmented Generation with Hypergraph-Structured Knowledge Representation")).

### 5. Temporal GraphRAG (T-GRAG)

T-GRAG incorporates temporal dynamics into GraphRAG by building time-stamped, evolving knowledge subgraphs, decomposing temporal queries, and using a multi-layer retriever to resolve temporal conflicts and redundancies in retrieval ([arXiv](https://arxiv.org/abs/2508.01680?utm_source=chatgpt.com "T-GRAG: A Dynamic GraphRAG Framework for Resolving Temporal Conflicts and Redundancy in Knowledge Retrieval")).

---

## Main Differences

1. **Graph Structure**
    
    - **Standard GraphRAG** uses simple node-edge knowledge graphs extracted from text ([Graph Database & Analytics](https://neo4j.com/blog/genai/what-is-graphrag/?utm_source=chatgpt.com "What Is GraphRAG?")).
        
    - **HyperGraphRAG** replaces edges with hyperedges to model n-ary relations, enabling richer relational semantics ([arXiv](https://arxiv.org/abs/2503.21322?utm_source=chatgpt.com "HyperGraphRAG: Retrieval-Augmented Generation with Hypergraph-Structured Knowledge Representation")).
        
    - **T-GRAG** augments graphs with timestamps, capturing the evolution of knowledge over time ([arXiv](https://arxiv.org/abs/2508.01680?utm_source=chatgpt.com "T-GRAG: A Dynamic GraphRAG Framework for Resolving Temporal Conflicts and Redundancy in Knowledge Retrieval")).
        
2. **Retrieval Mechanism**
    
    - **Template-based and Cypher-driven** patterns in Standard GraphRAG focus on pre-written graph queries and post-processing ([Graph Database & Analytics](https://neo4j.com/blog/developer/graphrag-field-guide-rag-patterns/?utm_source=chatgpt.com "GraphRAG Field Guide: Navigating the World of Advanced ...")).
        
    - **Graph Neural Networks** in GFM-RAG learn embeddings and retrieval strategies end-to-end on large-scale graph data ([arXiv](https://arxiv.org/abs/2502.01113?utm_source=chatgpt.com "GFM-RAG: Graph Foundation Model for Retrieval Augmented Generation")).
        
    - **U-Retrieval** in MedGraphRAG balances global context with precise indexing for medical QA ([arXiv](https://arxiv.org/abs/2408.04187?utm_source=chatgpt.com "Medical Graph RAG: Towards Safe Medical Large Language Model via Graph Retrieval-Augmented Generation")).
        
3. **Domain Specialization**
    
    - **MedGraphRAG** is explicitly designed for evidence-based medical responses, emphasizing safety and credibility ([arXiv](https://arxiv.org/abs/2408.04187?utm_source=chatgpt.com "Medical Graph RAG: Towards Safe Medical Large Language Model via Graph Retrieval-Augmented Generation")).
        
    - **GFM-RAG** and **HyperGraphRAG** aim for broad applicability across domains by improving generalizability or modeling capabilities ([arXiv](https://arxiv.org/abs/2502.01113?utm_source=chatgpt.com "GFM-RAG: Graph Foundation Model for Retrieval Augmented Generation")).
        
    - **T-GRAG** addresses temporal reasoning, making it ideal for financial, legal, or historical QA where timeliness matters ([arXiv](https://arxiv.org/abs/2508.01680?utm_source=chatgpt.com "T-GRAG: A Dynamic GraphRAG Framework for Resolving Temporal Conflicts and Redundancy in Knowledge Retrieval")).
        
4. **Modeling Complexity**
    
    - **HyperGraphRAG** increases modeling complexity to handle n-ary relations, at the cost of more intricate graph construction and retrieval pipelines ([arXiv](https://arxiv.org/abs/2503.21322?utm_source=chatgpt.com "HyperGraphRAG: Retrieval-Augmented Generation with Hypergraph-Structured Knowledge Representation")).
        
    - **Standard GraphRAG** remains simpler, relying on existing graph databases and vector fallbacks for broad coverage ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/?utm_source=chatgpt.com "Improving Retrieval Augmented Generation accuracy with ...")).
        
    - **GFM-RAG** introduces an additional model training phase but simplifies downstream application by eliminating fine-tuning needs ([arXiv](https://arxiv.org/abs/2502.01113?utm_source=chatgpt.com "GFM-RAG: Graph Foundation Model for Retrieval Augmented Generation")).
        

---

## Use Cases

- **Open-Domain QA and Multi-Hop Reasoning**  
    Standard GraphRAG excels at navigating entity relationships in encyclopedic or business data to answer complex queries ([microsoft.github.io](https://microsoft.github.io/graphrag/?utm_source=chatgpt.com "Welcome - GraphRAG")).
    
- **Medical Question Answering and Fact Checking**  
    MedGraphRAG ensures outputs are grounded in vetted medical sources, suitable for clinical decision support or patient-facing health assistants ([arXiv](https://arxiv.org/abs/2408.04187?utm_source=chatgpt.com "Medical Graph RAG: Towards Safe Medical Large Language Model via Graph Retrieval-Augmented Generation")).
    
- **Domain-Agnostic Retrieval without Fine-Tuning**  
    GFM-RAG’s foundation model supports plug-and-play retrieval across unseen datasets—ideal for organizations lacking resources for domain-specific retraining ([arXiv](https://arxiv.org/abs/2502.01113?utm_source=chatgpt.com "GFM-RAG: Graph Foundation Model for Retrieval Augmented Generation")).
    
- **Modeling Complex Event Relations**  
    HyperGraphRAG’s hyperedges enable precise representation of events or transactions involving multiple actors or elements, beneficial in legal, financial, or supply-chain contexts ([arXiv](https://arxiv.org/abs/2503.21322?utm_source=chatgpt.com "HyperGraphRAG: Retrieval-Augmented Generation with Hypergraph-Structured Knowledge Representation")).
    
- **Temporal Query Resolution**  
    T-GRAG’s temporal awareness allows it to answer “What was the CEO’s compensation in 2023 vs. 2024?” by filtering and reasoning over time-stamped graph snapshots ([arXiv](https://arxiv.org/abs/2508.01680?utm_source=chatgpt.com "T-GRAG: A Dynamic GraphRAG Framework for Resolving Temporal Conflicts and Redundancy in Knowledge Retrieval")).
    

---

**References to Key Sources:**

- Neo4j: “What Is GraphRAG?” ([Graph Database & Analytics](https://neo4j.com/blog/genai/what-is-graphrag/?utm_source=chatgpt.com "What Is GraphRAG?"))
    
- AWS: “Improving RAG Accuracy with GraphRAG” ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/?utm_source=chatgpt.com "Improving Retrieval Augmented Generation accuracy with ..."))
    
- Wikipedia: “Retrieval-augmented generation” ([Wikipédia](https://en.wikipedia.org/wiki/Retrieval-augmented_generation?utm_source=chatgpt.com "Retrieval-augmented generation"))
    
- ArXiv: GFM-RAG ([arXiv](https://arxiv.org/abs/2502.01113?utm_source=chatgpt.com "GFM-RAG: Graph Foundation Model for Retrieval Augmented Generation"))
    
- ArXiv: MedGraphRAG ([arXiv](https://arxiv.org/abs/2408.04187?utm_source=chatgpt.com "Medical Graph RAG: Towards Safe Medical Large Language Model via Graph Retrieval-Augmented Generation"))

---

Here’s the honest answer: there isn’t a single, canonical list. “GraphRAG” is an umbrella, and papers/products slice it a few different ways. In practice, teams end up using **six families** of GraphRAG, each with pretty clear trade-offs.

# The six practical “types”

1. **LLM-extracted KG (Microsoft-style “local + global”)**  
    Build a knowledge graph from raw text with an LLM, cluster it into communities, write community summaries (global), and also do neighborhood hops around entities (local). Great for both specific questions and “what are the main themes?” corpus questions. ([microsoft.github.io](https://microsoft.github.io/graphrag/?utm_source=chatgpt.com "Welcome - GraphRAG"), [arXiv](https://arxiv.org/html/2404.16130v1?utm_source=chatgpt.com "A Graph RAG Approach to Query-Focused Summarization"), [microsoft.com](https://www.microsoft.com/en-us/research/project/graphrag/?utm_source=chatgpt.com "Project GraphRAG - Microsoft Research"))
    
2. **Curated KG / DB-first GraphRAG (schema + Cypher/SPARQL)**  
    Start from an owned knowledge graph in a graph DB; retrieve with graph queries and composable patterns, sometimes training the LLM to generate correct Cypher/SPARQL. Best when you care about governance, explainability, and exact graph semantics. ([Graph Database & Analytics](https://go.neo4j.com/rs/710-RRC-335/images/Developers-Guide-GraphRAG.pdf?utm_source=chatgpt.com "GraphRAG"), [graphrag.com](https://graphrag.com/reference/?utm_source=chatgpt.com "GraphRAG Patterns Catalog"), [arXiv](https://arxiv.org/abs/2504.05478?utm_source=chatgpt.com "GraphRAFT: Retrieval Augmented Fine-Tuning for Knowledge Graphs on Graph Databases"))
    
3. **Hybrid Graph + Vector RAG**  
    Use graph retrieval when explicit relations exist; fall back to vector/keyword search for fuzzy matches. Often the most robust “day-1” enterprise pattern. LightRAG is a well-known instance that mixes levels of retrieval. ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/?utm_source=chatgpt.com "Improving Retrieval Augmented Generation accuracy with ..."), [arXiv](https://arxiv.org/abs/2410.05779?utm_source=chatgpt.com "LightRAG: Simple and Fast Retrieval-Augmented Generation"))
    
4. **Graph-embedding / GNN-assisted GraphRAG**  
    Learn graph representations (GNNs or graph embeddings) and retrieve/route based on structure + text. Useful when you need learning-based generalisation beyond hand-written traversals. (Common in research and in high-accuracy stacks.) ([NVIDIA Developer](https://developer.nvidia.com/blog/boosting-qa-accuracy-with-graphrag-using-pyg-and-graph-databases/?utm_source=chatgpt.com "Boosting Q&A Accuracy with GraphRAG Using PyG and ..."), [arXiv](https://arxiv.org/abs/2501.00309?utm_source=chatgpt.com "Retrieval-Augmented Generation with Graphs (GraphRAG)"))
    
5. **Temporal / Dynamic GraphRAG**  
    Model time explicitly (time-stamped edges/nodes), decompose temporal queries, and retrieve across evolving snapshots to avoid stale or contradictory context. Great for finance/legal/news/history questions. ([arXiv](https://arxiv.org/abs/2508.01680?utm_source=chatgpt.com "T-GRAG: A Dynamic GraphRAG Framework for Resolving Temporal Conflicts and Redundancy in Knowledge Retrieval"))
    
6. **Efficiency-optimised GraphRAG**  
    Same core ideas, but with lighter indexing and adaptive retrieval to cut cost/latency while keeping most of the accuracy. Handy when your corpus is big and budgets are not. ([arXiv](https://arxiv.org/abs/2505.24226?utm_source=chatgpt.com "E^2GraphRAG: Streamlining Graph-based RAG for High Efficiency and Effectiveness"))
    

_(Emerging: “learning-to-retrieve” variants that train the LLM policy to decide when/how to hop and fetch—e.g., GraphRAG-R1 with process-constrained RL.)_ ([arXiv](https://arxiv.org/abs/2507.23581?utm_source=chatgpt.com "GraphRAG-R1: Graph Retrieval-Augmented Generation with Process-Constrained Reinforcement Learning"))

---

# Key differences at a glance

|Type|How it retrieves|What you store|Strengths|Trade-offs|Typical use cases|
|---|---|---|---|---|---|
|LLM-extracted KG (local+global)|Community summaries (global) + neighborhood hops (local)|LLM-built KG + community hierarchy + summaries|Handles specific + “overview” questions; good synthesis|Costly indexing; summary freshness to manage|Enterprise reports, tech docs, research collections ([microsoft.github.io](https://microsoft.github.io/graphrag/?utm_source=chatgpt.com "Welcome - GraphRAG"), [arXiv](https://arxiv.org/html/2404.16130v1?utm_source=chatgpt.com "A Graph RAG Approach to Query-Focused Summarization"))|
|Curated KG / DB-first|Cypher/SPARQL patterns; programmatic traversals|Existing KG with schema/governance|Accurate, auditable, policy-friendly|Needs a maintained KG and query expertise|Compliance, risk, MDM, decision tracing ([Graph Database & Analytics](https://go.neo4j.com/rs/710-RRC-335/images/Developers-Guide-GraphRAG.pdf?utm_source=chatgpt.com "GraphRAG"), [arXiv](https://arxiv.org/abs/2504.05478?utm_source=chatgpt.com "GraphRAFT: Retrieval Augmented Fine-Tuning for Knowledge Graphs on Graph Databases"))|
|Hybrid Graph + Vector|Choose graph or vector per query; fuse results|KG + embeddings + text chunks|Robust across messy corpora|Two systems to run/tune|General enterprise QA, support bots ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/?utm_source=chatgpt.com "Improving Retrieval Augmented Generation accuracy with ..."), [arXiv](https://arxiv.org/abs/2410.05779?utm_source=chatgpt.com "LightRAG: Simple and Fast Retrieval-Augmented Generation"))|
|Graph-embedding / GNN|Learned graph/text embeddings|Graph features + learned models|Strong on multi-hop and generalisation|Training infra + MLOps|Scientific/tech domains, high-accuracy QA ([NVIDIA Developer](https://developer.nvidia.com/blog/boosting-qa-accuracy-with-graphrag-using-pyg-and-graph-databases/?utm_source=chatgpt.com "Boosting Q&A Accuracy with GraphRAG Using PyG and ..."), [arXiv](https://arxiv.org/abs/2501.00309?utm_source=chatgpt.com "Retrieval-Augmented Generation with Graphs (GraphRAG)"))|
|Temporal / Dynamic|Time-aware decomposition + layered retrievers|Time-stamped graphs/snapshots|Resolves “when” and drift|Extra modelling of time|Finance, legal, news, ESG timelines ([arXiv](https://arxiv.org/abs/2508.01680?utm_source=chatgpt.com "T-GRAG: A Dynamic GraphRAG Framework for Resolving Temporal Conflicts and Redundancy in Knowledge Retrieval"))|
|Efficiency-optimised|Adaptive retrieval; lean indexes|Summary trees + entity maps|Much cheaper/faster|Slight accuracy drop vs. full GraphRAG|Large corpora, cost-sensitive apps ([arXiv](https://arxiv.org/abs/2505.24226?utm_source=chatgpt.com "E^2GraphRAG: Streamlining Graph-based RAG for High Efficiency and Effectiveness"))|

---

# When to use which

- **Have (or want) a governed KG?** → Go **DB-first GraphRAG**; add **GraphRAFT-style** query generation if you want the LLM to emit Cypher/SPARQL. ([arXiv](https://arxiv.org/abs/2504.05478?utm_source=chatgpt.com "GraphRAFT: Retrieval Augmented Fine-Tuning for Knowledge Graphs on Graph Databases"))
    
- **Mixed, messy corpora and you just need it to work?** → **Hybrid Graph + Vector** (graph when relational; vector when fuzzy). ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/?utm_source=chatgpt.com "Improving Retrieval Augmented Generation accuracy with ..."))
    
- **Lots of “global” questions (“summarise the corpus”, “themes across X”)?** → **Microsoft-style local+global** (community summaries). ([arXiv](https://arxiv.org/html/2404.16130v1?utm_source=chatgpt.com "A Graph RAG Approach to Query-Focused Summarization"))
    
- **Time matters (who owned what _in 2023_ vs _2025_)?** → **Temporal GraphRAG**. ([arXiv](https://arxiv.org/abs/2508.01680?utm_source=chatgpt.com "T-GRAG: A Dynamic GraphRAG Framework for Resolving Temporal Conflicts and Redundancy in Knowledge Retrieval"))
    
- **Tight latency/cost budgets?** → **Efficiency-optimised** variants. ([arXiv](https://arxiv.org/abs/2505.24226?utm_source=chatgpt.com "E^2GraphRAG: Streamlining Graph-based RAG for High Efficiency and Effectiveness"))
    
- **Pushing SOTA reasoning?** → **Graph-embedding/GNN** or **RL-controlled GraphRAG**. ([NVIDIA Developer](https://developer.nvidia.com/blog/boosting-qa-accuracy-with-graphrag-using-pyg-and-graph-databases/?utm_source=chatgpt.com "Boosting Q&A Accuracy with GraphRAG Using PyG and ..."), [arXiv](https://arxiv.org/abs/2507.23581?utm_source=chatgpt.com "GraphRAG-R1: Graph Retrieval-Augmented Generation with Process-Constrained Reinforcement Learning"))
    

If you tell me your data shape (do you already have a KG? how fresh does it need to be? what’s your latency budget?), I can sketch an architecture and retrieval flow that fits your constraints.

---

Short version: that list isn’t “types of GraphRAGs” (as in whole architectures). It’s a **menu of retriever patterns** you can plug into a GraphRAG pipeline. Think of them as tools the system can pick from (manually or via an agent) to fetch the right _slice of the graph_ for a given question.

# What each retriever means (and when to use it)

- **Vector / Full-text / Spatial indexes**  
    Use embeddings, keywords, or geo indexes to find _starting points_ (seed nodes/docs). Great for cold-start when you don’t know the right nodes yet.  
    _Use when:_ the user query is fuzzy (“docs about flaky Kubernetes networking in EU West”).  
    _Watch for:_ false positives—re-rank before traversing.
    
- **Neighborhood Traversal (k-hop)**  
    Pull the local ego-network around one or more seed nodes to add context (owners, components, tickets, attached docs).  
    _Use when:_ you’ve identified the right entity but need its immediate context.  
    _Watch for:_ hop explosion—cap hops, degree, and node types.
    
- **Path Traversals**  
    Find paths between entities (e.g., “service A ↔ outage X”), then expand those paths slightly to collect evidence (changes, incidents, owners).  
    _Use when:_ the user asks “how is A related to B?” or you need multi-hop reasoning.  
    _Watch for:_ combinatorial blow-up—bound path length / count.
    
- **Global Queries (precomputed summaries)**  
    Prebuilt, cross-topic summaries (e.g., community/theme summaries à la Microsoft’s GraphRAG “global” layer).  
    _Use when:_ corpus-level questions (“What are the main outage themes in 2025?”).  
    _Watch for:_ staleness—schedule refresh, or blend with fresh nodes.
    
- **Query Templates**  
    Hand-crafted Cypher/SPARQL for recurring question classes (“Who owns service X?”, “Show incidents by severity last 30d”).  
    _Use when:_ governance/compliance matters; you need deterministic retrieval.  
    _Watch for:_ coverage gaps—pair with a fallback retriever.
    
- **Dynamic Cypher (Text2Cypher)**  
    Let an LLM emit Cypher/SPARQL from the natural-language question + schema hint.  
    _Use when:_ you want flexibility over a curated KG without writing every template.  
    _Watch for:_ query injection / dangerous ops—sandbox + allow-list + cost caps.
    
- **Agentic Traversal**  
    Treat each retriever as a tool. The LLM plans → runs a retriever → inspects results → decides next step (iterate) until it has enough context.  
    _Use when:_ questions are heterogeneous and require multi-step fetching.  
    _Watch for:_ latency and tool-use loops—set step/time budgets.
    
- **Graph-Embedding Retrievers**  
    Learn embeddings for nodes or ego-nets so you can “fuzzy match” structure + text (not just token semantics).  
    _Use when:_ you need similarity over topology (e.g., “incidents structurally like this one”).  
    _Watch for:_ training/ops complexity; keep a simple fallback path.
    

# How this differs from “types of GraphRAG”

Earlier we talked about _architectural families_ (DB-first, hybrid graph+vector, temporal, etc.). What you pasted is the **retrieval layer taxonomy**—mix-and-match components you can slot into any of those families. In practice, a production GraphRAG will chain several of these:

1. **Vector search** → seeds
    
2. **Neighborhood (1–2 hops)** → local evidence
    
3. **Path traversal** (if the question mentions two entities)
    
4. **Global summary** (if the question is corpus-level)
    
5. **Query template or Text2Cypher** for structured asks
    
6. **Re-rank + dedupe** → pass to the LLM for grounded generation
    

# A concrete mapping (your “internship_GraphRAG” vibe)

- User asks: _“Why did service A’s latency spike last week?”_
    
    1. Vector search: find incidents/alerts mentioning service A and “latency”.
        
    2. Neighborhood: owners, dependencies, recent changes (1 hop).
        
    3. Path: `A → depends_on → B → had_change → PR-123` last week.
        
    4. Template: “incidents by service & time window” for exact counts.
        
    5. Re-rank; hand a clean, timestamped bundle to the LLM.
        

---

I tried to open the **GraphRAG Pattern Catalog** you referenced (looks like graphrag.com / a Neo4j write-up), but I’m hitting a browsing issue on my side right now, so I couldn’t fetch and cite the exact page. If you drop the link you’re looking at, I’ll pull it in and cross-reference each bullet with the source and any gotchas they note (like Text2Cypher guardrails and global-summary refresh cadences).

---

Got it — that write-up is **a proposed taxonomy**, not a canon. It mixes one widely adopted architecture (Microsoft’s “local + global” GraphRAG) with several **domain/scope variants** that are either niche or very new. In other words: it’s trying to bucket the space, but the field doesn’t agree on “exactly five types.”

Here’s how it maps to what’s real vs. emerging:

- **“Standard Knowledge GraphRAG.”** Real and broadly used. This matches Microsoft’s approach: build an LLM-derived KG, do **local neighbourhood** retrieval for specific questions and **global/community summaries** for corpus-level questions. See the MSR paper and docs. ([arXiv](https://arxiv.org/abs/2404.16130?utm_source=chatgpt.com "From Local to Global: A Graph RAG Approach to Query-Focused Summarization"), [microsoft.github.io](https://microsoft.github.io/graphrag/?utm_source=chatgpt.com "Welcome to GraphRAG - GitHub Pages"), [microsoft.com](https://www.microsoft.com/en-us/research/project/graphrag/?utm_source=chatgpt.com "Project GraphRAG - Microsoft Research"))
    
- **“Medical GraphRAG.”** Plausible as a _domain-specific_ GraphRAG (lots of teams do regulated-domain KGs), but “MedGraphRAG” isn’t a community standard term. I couldn’t verify that exact paper title/ID from your footnote just now — if you have the link, share it and I’ll check.
    
- **“GFM-RAG (Graph Foundation Model RAG).”** Concept makes sense (use a graph foundation model/GNN to retrieve or reason), but again, not a settled “type.” I couldn’t confirm that specific arXiv entry from your list.
    
- **“HyperGraphRAG.”** Using hypergraphs for n-ary relations is a known idea; as a named RAG variant it looks _very_ new. Couldn’t verify the cited paper quickly.
    
- **“Temporal GraphRAG (T-GRAG).”** Time-aware retrieval is a real need; Microsoft’s “global” vs “local” doesn’t handle temporal drift by itself. But the T-GRAG paper ID you pasted appears brand-new and I couldn’t confirm it in a quick search.
    

What _is_ established across vendors is the **retrieval patterns** you’d compose inside any GraphRAG system (vector/full-text seeding → neighbourhood or path traversals → optional global summaries → query templates or Text2Cypher). Neo4j’s field guide and ecosystem docs catalogue these patterns; they’re the practical building blocks regardless of which “type” banner you fly. ([Graph Database & Analytics](https://neo4j.com/blog/developer/graphrag-field-guide-rag-patterns/?utm_source=chatgpt.com "GraphRAG Field Guide: Navigating the World of Advanced RAG Patterns"))

If you want a safer framing for a deck or design doc, use **axes instead of “types”**:

- **Graph structure:** property KG (mainstream) vs. hypergraph vs. temporal snapshots. Microsoft’s local/global fits here as “property KG + community summaries.” ([arXiv](https://arxiv.org/abs/2404.16130?utm_source=chatgpt.com "From Local to Global: A Graph RAG Approach to Query-Focused Summarization"))
    
- **Retriever strategy:** templates / Cypher/SPARQL, Text2Cypher, neighbourhood & path traversals, and global/community summaries. ([Graph Database & Analytics](https://neo4j.com/blog/developer/graphrag-field-guide-rag-patterns/?utm_source=chatgpt.com "GraphRAG Field Guide: Navigating the World of Advanced RAG Patterns"))
    
- **Domain specialisation:** medical, finance, legal, etc. (governance + schema + sources change, not the core mechanics).
    
- **Learning add-ons:** graph embeddings/GNNs for retrieval/reranking (emerging; stack-dependent).
    

If you drop the exact links for those arXiv items, I’ll verify them and update the table with status (“established / emerging / unverified”), plus concrete pros/cons and when to pick each. In the meantime, if you’re building today, the most reproducible, well-documented path is the **Microsoft local+global recipe** (for mixed local/corpus questions) or a **DB-first KG + query-template + Text2Cypher** setup (for governed data). ([arXiv](https://arxiv.org/abs/2404.16130?utm_source=chatgpt.com "From Local to Global: A Graph RAG Approach to Query-Focused Summarization"), [microsoft.github.io](https://microsoft.github.io/graphrag/?utm_source=chatgpt.com "Welcome to GraphRAG - GitHub Pages"))