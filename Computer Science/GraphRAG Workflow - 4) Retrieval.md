#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs

Alright, Stage 4 is the **engine room** of GraphRAG. Retrieval is where you turn a user question into the _right_ subgraph + supporting text, with enough evidence and not too much noise. Below is a practical, build-ready map of **all the main retrieval possibilities** and how to assemble them.

> Quick note: I intended to attach live citations (MS GraphRAG “local vs global”, Neo4j’s Field Guide, AWS’s GraphRAG post, Text2Cypher docs), but my browsing tool is erroring right now. I’ll happily wire in the sources if you want me to retry or share links you’re using.

---

# 4) Retrieval — what you’re solving

- **Find starting points** (entities/chunks/themes)
    
- **Expand** (paths, neighbourhoods, motifs, time windows)
    
- **Aggregate & answer pre-shapes** (counts, groups)
    
- **Package evidence** (nodes/edges/docs with provenance)
    
- **Do all that under budgets** (latency, cost, tokens)
    

You almost always mix **seeders** + **traversals/queries** + **global layer** + **fusion/rerank**.

---

# A) Seeders (how you find “where to start”)

1. **Lexical (BM25 / full-text)**
    
    - Great for names, IDs, numbers, exact terms.
        
    - Use analyzers (language, e-mail, keyword), filters (type, time).
        
    - Cheap, high-precision; misses paraphrases.
        
2. **Semantic (vector/embedding)**
    
    - Good for paraphrase and intent; use for questions that don’t name entities.
        
    - What to embed: **TextUnits** (doc chunks), **Entities**, (optionally) **Relationships** or **ego-nets**.
        
    - ANN index choices: HNSW (low latency), IVF_PQ (memory-efficient), DiskANN variants for very large sets.
        
3. **Hybrid seeders (lexical + vector)**
    
    - Run both and **fuse** (RRF / weighted normalisation). This is the day-1 default for enterprise QA.
        
4. **Entity linking / mention detection**
    
    - Map mentions in the question (“Service Alpha”, “PR-123”) to canonical nodes; use dictionaries + vectors + context rules.
        
5. **Global/corpus seeders**
    
    - **Community summaries** / topic reports (from clustering + LLM summaries). For “global” asks (“what are the main failure themes in 2025?”), jump here first, then drill down.
        
6. **Schema filters**
    
    - Hard pre-filters (labels, time windows, tenant/RBAC) to keep everything cheap and safe.
        

---

# B) Traversals & structured queries (what you do after seeding)

## 1) Neighbourhood traversal (k-hop)

- Expand from seeds with **caps**:
    
    - hop limit (k = 1–2 for speed)
        
    - **degree** limit per node (avoid hubs)
        
    - **label**/type allow-list (only edges that matter)
        
    - **time** filter (valid_from/valid_to within window)
        
- Use when: you have the right entity and need immediate context (owners, dependencies, attached incidents, evidence).
    

**Cypher sketch**

```cypher
MATCH (s:Service {name:$name})
CALL apoc.path.expandConfig(s, {
  minLevel: 1, maxLevel: 2,
  relationshipFilter: "DEPENDS_ON|OWNS>|HAS_INCIDENT>",
  labelFilter: "+Service|+Incident|+Person",
  bfs: true, uniqueness: "NODE_GLOBAL"
}) YIELD path
WITH path
WHERE all(r IN relationships(path) WHERE
  coalesce(r.valid_from, datetime("1900")) <= $asOf
  AND coalesce(r.valid_to, datetime("9999")) >= $asOf)
RETURN path LIMIT 200;
```

## 2) Path queries

- **Shortest path** (fast, single answer) or **bounded allSimplePaths** (more coverage, risk of explosion).
    
- Use for “how is A related to B?” and for **explanations** (show the path you cite).
    

```cypher
MATCH (a:Service {name:$a}),(b:Incident {id:$b})
MATCH p = shortestPath((a)-[:DEPENDS_ON|:HAS_INCIDENT*..4]-(b))
RETURN p;
```

## 3) Motif/metapath queries

- Pre-define path “shapes” (e.g., `Service → DEPENDS_ON → Service → HAS_INCIDENT`) and query them directly.
    
- Strong for multi-hop Qs that repeat in your domain (RCA, supply chain, fraud, clinical journeys).
    

## 4) Aggregations (counts, trends, groups)

- **Template queries** for tables: “incidents by severity per week for Service X”, “top owners by open P1s”.
    
- Often produced via **Query Templates** or **Text2Cypher**.
    

## 5) Temporal retrieval

- “As-of” filters on edges/nodes (valid-time), or snapshot labels.
    
- **Windowed expansion**: only traverse edges valid in `[t0, t1]`.
    
- **Drift handling**: when facts conflict across time, prefer the version closest to the query’s time, optionally return both with timestamps.
    

---

# C) Programmatic, Templates, and **Text2Cypher**

1. **Query templates (hand-curated)**
    

- For recurring asks with governance requirements.
    
- Deterministic, audited, fast to execute. Keep a library + parameter validator.
    

2. **Dynamic Cypher / Text2Cypher**
    

- LLM generates Cypher from the question + schema.
    
- Use when: flexible structured retrieval needed, but you don’t want to hand-write every template.
    

**Guardrails you must add**

- **Read-only** role; **allow-listed** labels/rel-types/functions.
    
- **Bounded results** (LIMIT, timeouts, row caps).
    
- **Schema doc** in the prompt (labels, rels, properties, examples).
    
- **Static sanitation** (reject writes, DDL, APOC side effects).
    
- **Dry-run validator** that parses the query and checks it against the allow-list before execution.
    

---

# D) Agentic traversal (planner/controller)

Instead of a single shot, the LLM (or a policy) **plans → calls a retriever → inspects → repeats** until it has enough evidence.

**Tools it can call**

- Seeders (lexical/vector/global)
    
- Neighbourhood/path/motif traversals
    
- Query templates
    
- **Text2Cypher** (as _one_ of the tools)
    
- “Fetch evidence” (TextUnits linked to nodes/edges)
    
- “Aggregate” (pre-built analytical queries)
    

**Guardrails**

- Step/time budgets, tool allow-lists, early-exit heuristics, cached results, telemetry for decisions.
    
- Great for messy, multi-step questions; expect higher latency—use only when needed (route simple asks to templates).
    

---

# E) Graph-embedding & GNN-assisted retrieval (optional, powerful)

- **Node/edge/ego-net embeddings** to retrieve by **topological similarity** (e.g., “incidents structurally like this one”).
    
- **GNN encoders** (GraphSAGE/GAT) trained on your graph to produce embeddings that respect structure.
    
- Combine with text embeddings via **score fusion** or **learned rerankers**.
    
- Costly to train/operate; worth it for high-accuracy domains (scientific, risk/fraud, dense relational data).
    

---

# F) Fusion, reranking, and context shaping

After you fetch candidate nodes/paths/chunks:

- **Normalise and fuse scores** (lexical + vector → RRF or weighted sum).
    
- **Cross-encoder reranker** (optional) over text _and_ structure features.
    
- **Structure-aware rerank**: boost results that match motifs, obey temporal windows, or have **shorter, stronger** explanation paths.
    
- **MMR / diversity**: avoid near-duplicates.
    
- **Bundle evidence**: for each retained item, keep `{node_ids, path, rels, doc_id, offsets, timestamps}` ready for the generator.
    

---

# G) Retrieval evaluation (don’t skip this)

- **Hit/Recall@k** against labelled gold evidence (nodes, edges, paths, or chunks).
    
- **Path correctness** vs. gold motifs; **temporal correctness** (valid-time satisfied).
    
- **Latency budgets** by question class (lookup vs. RCA).
    
- **Ablations**: lexical only vs. vector only vs. hybrid; with/without reranker; with/without global layer.
    

---

# H) Guardrails & ops check-list

- **Caps**: k (hops), degree per node, max paths, time window, rows, tokens.
    
- **Safety**: RBAC/tenancy filters in _every_ query; redact PII before prompting.
    
- **Caching**: seeds, paths, aggregates, and community-summary lookups.
    
- **Observability**: log each tool call, query text, parameters, timings, sizes; keep provenance for audit.
    

---

## Ready-to-use retrieval **recipes**

### 1) Entity lookup (“Who owns Service A?”)

- **Seeder**: lexical (exact name) → node ID
    
- **Query**: template or Text2Cypher for `(Service)-[:OWNS]-(Person)`
    
- **Evidence**: Service node, ownership edge, source TextUnits
    
- **Latency target**: sub-second to 2s
    

### 2) Path/relationship (“How is A related to incident X?”)

- **Seeder**: lexical/vector → `Service A`, `Incident X`
    
- **Query**: `shortestPath` (≤4) + small 1-hop expansion for context
    
- **Fusion**: prefer paths with valid-time overlap
    
- **Output**: show the path + citations
    

### 3) Root cause / “Why did latency spike last week?”

- **Seeder**: vector seeds (incidents/logs about latency + “Service A”)
    
- **Traversal**: time-windowed neighbourhood (A, its dependencies, recent Changes)
    
- **Path**: A → DEPENDS_ON → B → HAD_CHANGE(PR-123) within T
    
- **Aggregate**: template/Text2Cypher for incidents by component/time
    
- **Agentic**: allowed, with 4–6 step budget
    
- **Output**: hypothesis + linked evidence, times, owners
    

### 4) Global theme (“What were the main failure themes in 2025 Q2?”)

- **Seeder**: community summaries (global layer)
    
- **Drill-down**: for top communities, fetch exemplar incidents/changes + paths
    
- **Aggregate**: counts/trends per theme
    
- **Output**: bullets + per-theme citations
    

### 5) Similar case retrieval (“Find incidents like this one.”)

- **Seeder**: incident node → **ego-net embedding** or text+structure features
    
- **Retriever**: ANN over embeddings; filter by time/owner/region
    
- **Optional**: structure-aware rerank (shared dependencies/owners)
    

---

## Minimal interfaces (so your code stays clean)

```ts
type Seed = { kind: "entity"|"chunk"|"community", id: string, score: number };
type Evidence = {
  nodes: string[], edges: string[], paths?: string[][],
  chunks?: {docId: string, span: [number,number]}[],
  meta: {score: number, reasons: string[]}
};

interface Seeder {
  findSeeds(q: Query, k: number, opts): Promise<Seed[]>;
}
interface Traversal {
  expand(seeds: Seed[], opts): Promise<Evidence[]>;
}
interface Aggregator {
  run(params): Promise<Table | Series>;
}
interface Reranker {
  rerank(items: Evidence[]): Evidence[];
}
```

Wire a **router** that maps your _question archetypes_ (Stage 0) to:

- seeders to run,
    
- traversals/queries to apply,
    
- fusion/rerank rules,
    
- and a max **budget** (steps/time/tokens).
    

---

If you share your **question archetypes**, **schema**, and **SLOs**, I’ll turn this into concrete Cypher templates (incl. temporal filters), a **Text2Cypher guardrail prompt**, and an **agentic plan** (tools + step budgets) tailored to your graph. And if you want me to retry browsing, I’ll pull in the MS GraphRAG docs, Neo4j’s Field Guide, and AWS’s GraphRAG post to anchor each pattern with references.