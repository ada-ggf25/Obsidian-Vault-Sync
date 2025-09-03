#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs

Love that you want to start at the real beginning. If you get **Stage 0: Problem Framing & Data Scoping** right, every downstream GraphRAG decision gets easier (schema, retrievers, agentic loops, evals, guardrails… all of it). Here’s a tight, build-ready way to do it—with concrete choices you can actually make.

# Stage 0 — Problem Framing & Data Scoping

## 1) Outcomes & guardrails (write these first)

- **Primary tasks / question classes** you must support (e.g., lookup, list, relation/path, multi-hop reasoning, corpus-level themes, temporal “as-of”, numerical/table). Microsoft’s GraphRAG explicitly divides _local (entity-centric)_ vs _global (corpus-level)_ questions—use that split to ground your taxonomy. ([Microsoft GitHub](https://microsoft.github.io/graphrag/ "Welcome - GraphRAG"), [microsoft.com](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/ "GraphRAG: Unlocking LLM discovery on narrative private data - Microsoft Research"))
    
- **Acceptance criteria & metrics:** define how you’ll measure _correctness, groundedness/faithfulness, context precision/recall, readability_. Tooling like **RAGAS** and enterprise guides (Databricks) are commonly used to operationalize these before you build. ([GitHub](https://github.com/explodinggradients/ragas "GitHub - explodinggradients/ragas: Supercharge Your LLM Application Evaluations"), [Databricks](https://www.databricks.com/blog/LLM-auto-eval-best-practices-RAG "Best Practices for LLM Evaluation | Databricks Blog"))
    
- **Risk and compliance level:** classify the use case (internal helper vs. regulated advice) and decide documentation/provenance requirements up front. NIST’s AI Risk Management Framework gives a good template for “risk profile → controls.” ([NIST](https://www.nist.gov/itl/ai-risk-management-framework "AI Risk Management Framework | NIST"))
    

**Deliverable:** a one-pager stating: _tasks in scope_, _must-have metrics_, _how you’ll measure them_, _risk level & evidence requirements_.

---

## 2) Question archetypes & routing plan

Create a shortlist of archetypes you’ll support **now** (and defer the rest). For each, capture: _example queries, retrieval style, and answer format_.

- **Local / entity-specific Q&A** → seed → small k-hop context (owners, dependencies, attached docs). ([Microsoft GitHub](https://microsoft.github.io/graphrag/ "Welcome - GraphRAG"))
    
- **Global / corpus-level themes** → community summaries / query-focused summarization. ([Microsoft GitHub](https://microsoft.github.io/graphrag/ "Welcome - GraphRAG"), [microsoft.com](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/ "GraphRAG: Unlocking LLM discovery on narrative private data - Microsoft Research"))
    
- **Relation/path (“How is A related to B?”)** → bounded path search + small neighbourhood expansion. (Neo4j’s field guide frames these as distinct retrieval patterns you can compose.) ([Graph Database & Analytics](https://neo4j.com/blog/developer/graphrag-field-guide-rag-patterns/ "GraphRAG Field Guide: Navigating the World of Advanced RAG Patterns"))
    
- **Temporal (“as of June 2024”, “compare 2023 vs 2024”)** → require time scoping & conflict handling; plan snapshots or valid-time properties. (AWS’ GraphRAG write-up even evaluates _temporal_ question types.) ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/ "Improving Retrieval Augmented Generation accuracy with GraphRAG | Artificial Intelligence"))
    
- **Ambiguous questions → clarify first.** Pre-retrieval clarifications (“Tree of Clarifications”) are a known win before you burn cycles pulling context. Use them where needed. ([ar5iv](https://ar5iv.org/pdf/2312.10997 "[2312.10997] Retrieval-Augmented Generation for Large Language Models: A Survey"))
    

**Deliverable:** a routing table: _archetype → retriever(s) → answer format → metrics that matter._

---

## 3) Data inventory & scope (what’s in, what’s out)

- **Sources:** list structured (DBs, catalogs), semi-structured (CSVs, PDFs), unstructured (docs, email, tickets), logs/events.
    
- **Truth & provenance:** decide _sources of truth_ and minimum provenance you need to store per fact/chunk (doc ID, offsets, timestamps). Microsoft’s GraphRAG emphasizes provenance for verification and trust. ([microsoft.com](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/ "GraphRAG: Unlocking LLM discovery on narrative private data - Microsoft Research"))
    
- **Access constraints:** tenants, RBAC, redaction.
    
- **Out of scope:** make an explicit “not now” list so you can ship.
    

**Deliverable:** a data map with owners, update cadences, and allowed use.

---

## 4) Freshness & temporal policy

- **Staleness tolerance:** e.g., “answers must reflect the last 7 days by default unless the user asks _as of_ a date.”
    
- **Temporal modeling choice:** snapshots vs. valid-time properties vs. bi-temporal (you’ll decide the exact modeling in Stage 1, but scope it now). AWS’ overview shows teams doing exactly this for GraphRAG pipelines. ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/ "Improving Retrieval Augmented Generation accuracy with GraphRAG | Artificial Intelligence"))
    

**Deliverable:** freshness SLOs and the default time window per question class.

---

## 5) Evidence & explainability requirements

- **Citations:** inline node/edge/doc references—mandatory or optional?
    
- **Trace granularity:** path lists, community summaries, or raw text snippets? Microsoft’s blog calls out provenance as a first-class requirement; note your minimum acceptable standard. ([microsoft.com](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/ "GraphRAG: Unlocking LLM discovery on narrative private data - Microsoft Research"))
    

**Deliverable:** a citation policy (what to cite and how).

---

## 6) Performance & budget envelopes

- **Latency targets:** e.g., P95 ≤ 3s for lookup, ≤ 8s for investigations.
    
- **Cost ceilings:** max tokens per answer; cap retriever steps for agentic flows.
    
- **Throughput expectations:** concurrent users, workload mix.
    

**Deliverable:** an SLO sheet (latency, throughput, token/cost budgets by archetype).

---

## 7) Safety, privacy & governance (set the bar now)

- **PII / sensitive data handling:** redaction/masking rules and prompt hygiene (no secrets in system prompts).
    
- **Schema/constraints:** if you’ll use RDF/SHACL for validation, note it now so Stage 1 modeling aligns. (SHACL is the W3C standard for validating RDF graphs against shapes/constraints.) ([W3C](https://www.w3.org/TR/shacl/ "Shapes Constraint Language (SHACL)"))
    
- **Auditability:** logging of evidence used; read-only DB roles for generated queries.
    

**Deliverable:** a one-pager: what’s allowed, masked, logged, and by whom.

---

## 8) Evaluation plan (decide before you build)

- **Golden sets** per archetype (10–50 Q/A each to start).
    
- **Metrics & tooling:** faithfulness/groundedness, context precision/recall (RAGAS), and human/LLM-as-judge where appropriate (Databricks’ practical guidance). ([GitHub](https://github.com/explodinggradients/ragas "GitHub - explodinggradients/ragas: Supercharge Your LLM Application Evaluations"), [Databricks](https://www.databricks.com/blog/LLM-auto-eval-best-practices-RAG "Best Practices for LLM Evaluation | Databricks Blog"))
    
- **Gates:** what metric thresholds unblock launch.
    

**Deliverable:** an eval README with datasets, metrics, and pass/fail bars.

---

## 9) UX boundaries

- **When to ask clarifying questions**, when to refuse, how to phrase uncertainty.
    
- **Answer shapes:** paragraph, bullet summary, table/JSON; per archetype.
    
- **Escalation paths:** when to show raw sources or prompt the user to narrow.
    

**Deliverable:** UX rules by archetype (incl. clarification prompts for ambiguous queries). ([ar5iv](https://ar5iv.org/pdf/2312.10997 "[2312.10997] Retrieval-Augmented Generation for Large Language Models: A Survey"))

---

# How Stage 0 folds into later choices

- If **global questions** matter → you’ll plan for **community detection + summaries** (Microsoft’s “global” mode). ([Microsoft GitHub](https://microsoft.github.io/graphrag/ "Welcome - GraphRAG"))
    
- If **temporal queries** are common → choose **valid-time edges** or **snapshots** and default time windows. ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/ "Improving Retrieval Augmented Generation accuracy with GraphRAG | Artificial Intelligence"))
    
- If **provenance is mandatory** → ensure your retrievers preserve path/evidence and your prompts _require_ citations (MS GraphRAG shows how provenance improves trust). ([microsoft.com](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/ "GraphRAG: Unlocking LLM discovery on narrative private data - Microsoft Research"))
    
- If **risk is high** → favor **templates/Text2Cypher** over free-form agents; add SHACL/constraints where applicable. ([Graph Database & Analytics](https://neo4j.com/blog/developer/graphrag-field-guide-rag-patterns/ "GraphRAG Field Guide: Navigating the World of Advanced RAG Patterns"), [W3C](https://www.w3.org/TR/shacl/ "Shapes Constraint Language (SHACL)"))
    

---

## Quick checklist you can copy into your doc

- **Tasks in scope:** (local lookup, path, global themes, temporal compare, numeric/table) → examples included. ([Microsoft GitHub](https://microsoft.github.io/graphrag/ "Welcome - GraphRAG"), [Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/ "Improving Retrieval Augmented Generation accuracy with GraphRAG | Artificial Intelligence"))
    
- **Success metrics & eval plan:** RAGAS metrics + small golden sets; LLM-as-judge rubric. ([GitHub](https://github.com/explodinggradients/ragas "GitHub - explodinggradients/ragas: Supercharge Your LLM Application Evaluations"), [Databricks](https://www.databricks.com/blog/LLM-auto-eval-best-practices-RAG "Best Practices for LLM Evaluation | Databricks Blog"))
    
- **Risk profile & controls:** provenance level, RBAC, masking/redaction, SHACL (if RDF). ([W3C](https://www.w3.org/TR/shacl/ "Shapes Constraint Language (SHACL)"), [microsoft.com](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/ "GraphRAG: Unlocking LLM discovery on narrative private data - Microsoft Research"))
    
- **Freshness policy:** default window, “as-of” support. ([Amazon Web Services, Inc.](https://aws.amazon.com/blogs/machine-learning/improving-retrieval-augmented-generation-accuracy-with-graphrag/ "Improving Retrieval Augmented Generation accuracy with GraphRAG | Artificial Intelligence"))
    
- **SLOs & budgets:** latency, max steps, token caps per archetype.
    
- **UX rules:** clarifications policy and answer formats per archetype. ([ar5iv](https://ar5iv.org/pdf/2312.10997 "[2312.10997] Retrieval-Augmented Generation for Large Language Models: A Survey"))
    

If you want, drop your **real** question archetypes (5–8 examples) and the **data sources** you have. I’ll turn this into a Stage-0 worksheet tailored to your project and show how it maps into the indexing/query patterns from Microsoft GraphRAG and Neo4j’s retriever catalog. ([Microsoft GitHub](https://microsoft.github.io/graphrag/ "Welcome - GraphRAG"), [Graph Database & Analytics](https://neo4j.com/blog/developer/graphrag-field-guide-rag-patterns/ "GraphRAG Field Guide: Navigating the World of Advanced RAG Patterns"))