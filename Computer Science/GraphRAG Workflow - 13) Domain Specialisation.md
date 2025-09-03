#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs

Awesome—**Stage 13: Domain Specialisation** is where a generic GraphRAG becomes _fit for purpose_ in healthcare, finance, legal, cyber, etc. The trick is: don’t invent a new architecture per domain—compose the same GraphRAG building blocks (local/global/DRIFT, retriever patterns, templates/Text2Cypher) with domain-specific **schema, sources, guardrails, query patterns, and evals**.

Below is a practical blueprint you can lift into a design doc, with concrete choices and where they plug in.

---

# What “domain specialisation” actually changes

- **Knowledge model:** entities/relations, plus event/statement nodes and time fields suited to the domain.
    
- **Ingestion rules:** which sources you trust and how you extract/validate claims.
    
- **Retrieval mix:** templates, traversals, global/community use, Text2Cypher guardrails.
    
- **Generation constraints:** citation rigor, disclaimers, answer formats (tables vs prose).
    
- **Evals:** domain-specific gold sets & metrics.
    

Microsoft’s GraphRAG cleanly separates **indexing artifacts** (TextUnits, Entities/Relationships, _community summaries_) and **query modes**—**Local** (entity-centric), **Global** (corpus-level via community summaries), and **DRIFT** (mix of both). That’s your skeleton; you swap in domain pieces on top.

Neo4j’s field guide catalogs retrieval **patterns** you’ll keep reusing—query templates, dynamic/Text2Cypher, neighbourhood/path traversals, and a **Global Community Summary Retriever** pattern. That gives you a menu to mix per domain.

AWS’s GraphRAG write-up also shows why this pays off in real industries (finance/healthcare/law/industry), and even points to managed GraphRAG support with Neptune/Bedrock.

---

# A domain-specialisation methodology (copy this workflow)

## 1) Scope & risk profile

- **Question classes** in this domain (lookup, path/relationship, temporal as-of/compare, trends, RCA).
    
- **Regulatory posture**: citations mandatory? human-in-the-loop? allowed sources?
    

Map those to GraphRAG query modes: **Global** for thematic questions, **Local** for entity/path asks, **DRIFT** when the ask is broad but needs specific evidence.

## 2) Domain schema & vocabularies

- Draft a **property-graph schema** (or RDF) with event/statement nodes for n-ary facts + `valid_from/valid_to`.
    
- Pick **controlled vocabularies** (codes, taxonomies) and normalise into the graph at ingest so queries/filters are deterministic.
    
- Decide where you’ll keep **community membership** + **community summaries** for global questions. (These are first-class artifacts in GraphRAG.)
    

## 3) Source curation & claim policy

- Enumerate **trusted sources** (structured DBs, filings, standards docs, vetted websites, internal systems).
    
- For each source, set **claim templates** (what entities/relations it’s allowed to assert) and **provenance requirements** (doc → TextUnit → claim/edge).
    
- Add HITL or dual-LLM verification for safety-critical domains.
    

## 4) Retrieval kit (per domain)

Pick from the field-guide patterns: **query templates** for high-governance flows, **Text2Cypher** (guarded) for the long tail, **neighbourhood/path** traversals for reasoning, **Global Community Summary Retriever** for corpus asks.

## 5) Generation rules & formats

- Grounded answers only; inline citations to **graph paths** and **TextUnit spans**.
    
- Domain-specific outputs (JSON tables, timelines, cohort summaries) with structured outputs.
    
- If ambiguous, **clarify** before running heavy retrieval.
    

## 6) Evaluation suite

- Gold questions per domain (10–50 to start): include expected entities/paths, text spans, and if applicable, **expected communities** for global questions.
    
- Track: **faithfulness/groundedness**, **context precision/recall**, **path/temporal correctness**, and **latency/cost**; test Local vs Global vs **DRIFT** separately.
    

---

# Domain playbooks (what to specialise, and how)

## Healthcare (safety-critical)

**Model:** Patients, conditions, procedures, medications, encounters; **event nodes** for clinical events; temporal fields everywhere.  
**Sources:** EHR extracts, clinical guidelines, formulary; keep strong provenance and disclaimers.  
**Retrieval:**

- **Templates** for counts/temporal cohorts (encounters, labs by window).
    
- **Neighbourhood** around a patient/problem list; **path** for care journeys.
    
- **Global** summaries for population-level patterns; use **DRIFT** when a patient-specific ask also needs population norms.  
    **Guardrails:** read-only templates for any numerics; abstain if evidence isn’t from vetted sources.  
    **Eval:** faithfulness + temporal correctness; clinician review loop.
    

## Finance / Public filings

**Model:** Issuers, instruments, statements/line items as **events/claims** with period dates; relationships to subsidiaries, deals, and risks.  
**Sources:** SEC/EDGAR filings, market data, internal ledger; snapshot by period.  
**Retrieval:**

- **Templates** for line-item retrieval and period comparisons.
    
- **Path** queries for ownership/control chains; **temporal** filters to “as of FY2024”.
    
- **Global**: community summaries for recurring themes (revenue drivers, risk sections); **DRIFT** for “broad question + specific figures.”  
    **Eval:** numeric correctness from templates; time-scoped answers.
    

## Legal / Policy

**Model:** Statutes/sections, cases, citations (edge type = “cites/overrules/applies_to”), **event** nodes for rulings and amendments; `valid_from/valid_to`.  
**Sources:** Official codes, court opinions, regulator guidance; provenance mandatory.  
**Retrieval:**

- **Path** along citation chains; **motifs** like “section → cited_by → case → holding”.
    
- **Global** summaries for topic overviews; **Local** for “does §X apply?”
    
- **Text2Cypher** (guarded) for count/coverage questions when templates don’t fit.  
    **Generation:** neutral tone + precise citations; abstain when jurisdiction/time unclear.
    

## Cybersecurity / Threat intel

**Model:** Entities for software, CVEs, vendors; **events** for exploits/incidents; edges for “uses tactic/technique”, “affects version”.  
**Sources:** CVE/NVD feeds, internal tickets, incident reports; keep timestamps and environment.  
**Retrieval:**

- **Templates** for vulnerability impact by asset class/time.
    
- **Path**: asset → depends_on → package → CVE; **temporal** to check if vulnerable “as of” now.
    
- **Global** summaries for top ATT&CK-style techniques in your env; **DRIFT** when an incident needs both local detail and global theme context.  
    **Eval:** timeliness (no stale CVE), path correctness, groundedness.
    

## Manufacturing / Supply chain

**Model:** Products/BOM, suppliers, shipments; **events** for POs, shipments, QC failures.  
**Retrieval:**

- **Path** traversal across tiers; **temporal** deltas “what changed between rev A and B”.
    
- **Global**: community summaries for recurring failure modes; **Local** drill-down for a specific SKU.
    

> You’ll notice the **pattern doesn’t change**: you swap in domain entities/sources, choose templates vs Text2Cypher, control time, and decide when to leverage **Global** vs **Local/DRIFT**. That’s exactly how the official GraphRAG docs intend the system to be specialised.

---

# Concrete deliverables (per domain “plugin”)

1. **Schema pack**
    
    - Node/edge types, required props, `valid_from/valid_to`, event/statement shapes.
        
2. **Source pack**
    
    - Whitelist of sources, parsers, extraction JSON schemas, provenance requirements.
        
3. **Retriever pack**
    
    - n–m **Query Templates** (Cypher/SPARQL), **Text2Cypher** allow-list, motif/path snippets, temporal defaults. (Neo4j’s guide lists these patterns and how to implement them.)
        
4. **Global pack**
    
    - Community detection params, **community summary** prompt, refresh cadence; DRIFT threshold for when to use global cues.
        
5. **Generation pack**
    
    - Domain answer styles (table/JSON/prose), citation format, disclaimers.
        
6. **Eval pack**
    
    - 10–50 gold Qs, expected entities/paths/spans, mode (Local/Global/DRIFT), pass/fail thresholds.
        
7. **Ops & guardrails**
    
    - Read-only DB role, label/relationship allow-list, row/time caps for Text2Cypher; provenance logging. (AWS and Neo4j both stress managed/guarded GraphRAG patterns.)
        

---

# Starter checklist to stand up a new domain in <2 weeks

- **Day 1–2:** lock **question classes** + risk posture; draft schema/events/time fields.
    
- **Day 3–5:** ingest 1–2 trusted sources → TextUnits + Entities/Relationships + 2–3 event types; build 6–10 **templates** for common asks.
    
- **Day 6–7:** enable **Local** and **Global** (community summaries) + a minimal DRIFT rule.
    
- **Day 8–9:** wire **guarded Text2Cypher** for long-tail structured asks; add 2–3 path/motif traversals.
    
- **Day 10–14:** build a **gold set** (15–30 Qs), run offline evals (faithfulness, context precision/recall, path/temporal), iterate prompts & templates; log provenance everywhere.
    
- **Ongoing:** schedule **community summary** refreshes; add DRIFT for broad → specific questions; measure accuracy/latency/cost; expand sources.
    

---

If you tell me the **domain** you’re tackling first (and a couple of example questions), I’ll sketch the schema, top-5 query templates, a guarded Text2Cypher prompt, and the global/DRIFT settings tuned for that domain—grounded in the official GraphRAG modes and the retriever patterns Neo4j/AWS document.