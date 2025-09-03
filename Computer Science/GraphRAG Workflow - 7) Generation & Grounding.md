#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs

Alright—Stage 7 is where your system turns the retrieved subgraph + texts into a **grounded, auditable answer** (and avoids inventing stuff). Here’s how to design **Generation & Grounding** so it’s reliable, cheap, and explainable.

# What “Generation & Grounding” must achieve

1. **Write from evidence, not memory** (only use what retrieval returned).
    
2. **Show receipts** (inline citations to nodes/edges/text spans).
    
3. **Handle global vs local questions** (summaries vs entity-centric facts).
    
4. **Fail safely** (abstain or ask to clarify when evidence is thin).  
    Microsoft’s GraphRAG formalises this split—_Global_ answers draw on community summaries; _Local_ answers use neighbourhood facts; **DRIFT** blends both at query time.
    

---

# A. Output styles you should support

- **Grounded narrative** (paragraphs with inline citations to graph paths + text spans).
    
- **Table/JSON** (counts, grouped stats, entity lists) with per-row provenance.
    
- **Global summaries** (query-focused summarisation over community reports).  
    GraphRAG ships community reports, entities, relationships, and text units as first-class outputs you can cite directly.
    

---

# B. Prompting patterns that cut hallucinations

## 1) Grounded QA (local)

Use a strict “only from evidence” style:

- _Rules:_ “Use the EVIDENCE only. If a claim is not supported, say you don’t know. Cite node IDs/paths and source spans.”
    
- _Inputs:_ (a) **graph paths** (IDs, edge types, valid_from/to), (b) **text spans** (doc id + offsets), (c) **query intent**.  
    This mirrors GraphRAG’s local mode where answers are derived from entity neighbourhoods and their supporting TextUnits.
    

## 2) QFS (global / corpus-level)

Summarise **community reports** relevant to the question, then synthesize a final answer; keep citations to the contributing communities and exemplar text spans. This is exactly the approach introduced for GraphRAG’s **Query-Focused Summarization** (QFS).

## 3) DRIFT (mixed)

Start from global (themes) → drill down to local (entities/paths) → compose. Use when the question is broad but needs specific facts.

---

# C. Multi-pass generation (cheap and reliable)

1. **Draft → Verify → Final**
    
    - **Draft:** produce an answer plan + the set of claims and which evidence items support each one.
        
    - **Verify:** for each claim, ensure at least one evidence item (path or text span) is attached; drop or mark anything unsupported.
        
    - **Final:** write the answer with only the verified claims, plus citations.
        
2. **Answer shaping**
    
    - **Local:** prefer short paths, time-valid edges, and direct quotes from TextUnits.
        
    - **Global:** stitch together community-level bullets first, then cite exemplar nodes/docs.  
        GraphRAG’s materials emphasise provenance via TextUnits and community reports—your verify pass should fail any claim that can’t point to these artefacts.
        

---

# D. Structured outputs & constrained decoding

When the user wants a table/JSON, don’t let the model “free-write”. Use **structured outputs** with a JSON Schema and reject anything non-conforming. (You can also expose tools for aggregation and let the model call them; keep queries read-only.)

**Good patterns**

- **JSON Schema (Structured Outputs):** enforce types, required fields, and citation fields.
    
- **Function/tool calling:** route the model to pre-built aggregate queries (e.g., “incidents_by_week(service, window)”).
    

---

# E. Numerics, comparisons, and temporal grounding

- **Never compute from memory.** For numbers (counts, averages, timelines), call a **governed aggregator** (query template or Text2Cypher) and then render the result.
    
- **As-of dates:** always include “as of ” for time-scoped answers and prefer evidence closest to the specified date.  
    GraphRAG’s local mode already works over time-scoped entity neighbourhoods; DRIFT helps when a mix of thematic context and dated facts is needed.
    

---

# F. Handling conflicting or thin evidence

- **Conflict:** present both facts with timestamps + sources; say which is newer/closer to the asked date.
    
- **Thin evidence:** return a partial answer with an explicit “insufficient evidence” note and offer clarifying follow-ups (GraphRAG includes question-generation utilities for follow-ups).
    

---

# G. Citation formatting (make audits painless)

Include both **graph** and **text** provenance:

- **Graph:** `(node_id) --[REL, valid_from→valid_to]--> (node_id)`
    
- **Text:** `(doc_id, start_offset, end_offset)`  
    GraphRAG’s output schemas (entities, relationships, text_units, community_reports) are designed so you can reference them in answers and even show human-readable short IDs.
    

---

# H. Model choices & reranking hooks during generation

- **Global answers** benefit from smaller, per-community drafts then a final synthesis (QFS pipeline); this scales better than stuffing everything into one prompt.
    
- **Local answers** should keep the context window tight (top-k text spans, shortest valid paths). GraphRAG’s query docs show adjustable top-k txt units and question generation for follow-ups.
    

---

# I. Minimal, reusable prompt shapes

**Local (grounded QA)**

```
SYSTEM: You answer ONLY using EVIDENCE. 
- If a claim isn’t supported, say “Not enough evidence.”
- Cite each claim with (node_ids / edge types / doc_id:offsets).
- Prefer facts valid at ${AS_OF}.

USER: ${QUESTION}

EVIDENCE:
Graph paths: ${GRAPH_PATHS_JSON}
Text spans:  ${TEXT_SPANS_JSON}
```

(Placeholders are exactly the GraphRAG artefacts your pipeline produces.)

**Global (QFS)**

```
SYSTEM: Synthesize an answer from the COMMUNITY REPORTS below.
- Keep bullets grouped by theme; cite community ids and exemplar docs.
- If a theme lacks direct evidence, omit it.

USER: ${QUESTION}

COMMUNITY REPORTS: ${REPORT_EXCERPTS}
OPTIONAL EXEMPLARS: ${EXEMPLAR_TEXT_SPANS}
```

This mirrors the community-summary → synthesis flow in GraphRAG’s QFS work.

**Structured (JSON)**

```
SYSTEM: Produce JSON that matches this schema. No extra fields.
SCHEMA: ${JSON_SCHEMA}
EVIDENCE: ${AGGREGATE_RESULTS_WITH_CITATIONS}
```

Use a _structured outputs_ API so invalid JSON is rejected automatically.

---

# J. Putting it together (reference flow)

1. **Pick mode** (local | global | DRIFT) based on router/planner.
    
2. **Shape context**
    
    - Local: k shortest time-valid paths + top-k text spans.
        
    - Global: top communities’ reports (+ exemplars).
        
3. **Generate (draft) → verify → final** (drop unsupported claims).
    
4. **Emit citations** using GraphRAG IDs/short IDs from the output tables.
    
5. **For tables/JSON:** call aggregators first; then generate via structured output.
    

---

# K. What to log for QA

- Evidence IDs used, claim→evidence mapping, abstentions, mode (local/global/DRIFT), token counts.  
    GraphRAG provides notebooks and API examples that show how to wire these artefacts through your pipeline.
    

---

If you want, share one **local** question and one **global** question from your domain; I’ll draft the exact prompts (grounded + QFS), define the JSON schemas for structured outputs, and show how to assemble the evidence bundle from your GraphRAG artefacts.