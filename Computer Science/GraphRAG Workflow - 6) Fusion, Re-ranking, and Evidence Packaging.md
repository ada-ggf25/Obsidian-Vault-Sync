#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs

Let’s make Stage 6 concrete. This is the part where you _blend_ multiple retrieval signals, _re-rank_ what you fetched, and then _package_ the final, grounded evidence for the generator. Think: **quality-in → quality-out**.

# What “Fusion, Re-ranking, and Evidence Packaging” really do

- **Fusion**: combine lexical (BM25), semantic (vector), and graph-structure signals into one list. Azure’s hybrid search is a canonical example: it merges vector and text results with **Reciprocal Rank Fusion (RRF)**. ([Microsoft Learn](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview?utm_source=chatgpt.com "Hybrid search - Azure AI Search | Microsoft Learn"))
    
- **Re-ranking**: push the _most_ relevant/faithful items to the top (often with a stronger model than you used for first-stage retrieval). This can be a cross-encoder/LLM reranker or a structure-aware policy. ([arXiv](https://arxiv.org/abs/2212.10528?utm_source=chatgpt.com "HYRR: Hybrid Infused Reranking for Passage Retrieval"))
    
- **Evidence packaging**: deliver a clean, compact **bundle** (subgraphs, paths, and source text spans with timestamps) to the LLM. The Microsoft GraphRAG docs make this explicit with **TextUnits**, **Entities/Relationships**, and **Community Reports**. ([Microsoft GitHub](https://microsoft.github.io/graphrag/index/overview/?utm_source=chatgpt.com "Overview - GraphRAG"))
    

---

# A) Fusion: options that actually work

## 1) Reciprocal Rank Fusion (RRF) — strong, simple default

- Run multiple queries (e.g., BM25 + vector), then fuse by rank with RRF (score ≈ Σ 1/(rank+k)). Azure AI Search uses this by default for hybrid queries; it also supports “semantic reranking” _after_ RRF. Use RRF when you want robust, parameter-light fusion. ([Microsoft Learn](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview?utm_source=chatgpt.com "Hybrid search - Azure AI Search | Microsoft Learn"))
    

## 2) Weighted score / Convex Combination (learn a blend)

- Normalize scores (z-score/min–max) and learn a **single weight** to mix lexical + vector (sometimes beats RRF with a tiny labeled set). Recent analyses show learned convex combinations can outperform RRF across domains if you can tune that one knob. ([arXiv](https://arxiv.org/abs/2210.11934?utm_source=chatgpt.com "An Analysis of Fusion Functions for Hybrid Retrieval"))
    

## 3) Multi-signal & advanced hybrids

- You can fuse >2 lists (e.g., BM25 + dense + sparse + global/community seeds). Azure documents multi-query fusion, and research is exploring tensor/feature-rich fusions as well. Use when you have several competent retrieval paths and want a stable aggregate. ([Documentação Azure](https://docs.azure.cn/en-us/search/hybrid-search-how-to-query?utm_source=chatgpt.com "Hybrid query - Azure AI Search | Azure Docs"), [arXiv](https://arxiv.org/abs/2508.01405?utm_source=chatgpt.com "Balancing the Blend: An Experimental Analysis of Trade-offs in Hybrid Search"))
    

**Practical tip:** start with **RRF**; if you have a small validation set, try a **one-parameter convex blend** and keep whichever wins on your gold questions. ([arXiv](https://arxiv.org/abs/2210.11934?utm_source=chatgpt.com "An Analysis of Fusion Functions for Hybrid Retrieval"))

---

# B) Re-ranking: from “good” to “the best”

## 1) Cross-encoder / LLM rerankers (text-focused)

- Pass each (query, candidate) pair through a stronger model that attends to both together (e.g., BERT/T5/LLM as reranker). This consistently lifts precision@k in RAG stacks. HYRR and **Fusion-in-T5** are representative approaches. Azure also applies a _semantic reranker_ after RRF. ([arXiv](https://arxiv.org/abs/2212.10528?utm_source=chatgpt.com "HYRR: Hybrid Infused Reranking for Passage Retrieval"), [Microsoft Learn](https://learn.microsoft.com/pt-pt/azure/search/hybrid-search-ranking?utm_source=chatgpt.com "Pontuação de pesquisa híbrida (RRF) - Azure AI Search"))
    
- Use for **passage selection** from TextUnits when wording matters.
    

## 2) Structure-aware re-rank (GraphRAG-specific)

Boost items that:

- satisfy a **motif/metapath** you care about,
    
- have **short, high-confidence explanation paths**,
    
- are **temporally valid** for the user’s time window,
    
- or align with **global (community) themes** for corpus-level asks.  
    Recent GraphRAG work also explores _filtering/integration_ to suppress noisy graph facts and balance retrieved knowledge with the LLM’s own reasoning. ([Microsoft GitHub](https://microsoft.github.io/graphrag/query/overview/?utm_source=chatgpt.com "Overview - GraphRAG"), [arXiv](https://arxiv.org/abs/2503.13804?utm_source=chatgpt.com "Empowering GraphRAG with Knowledge Filtering and Integration"))
    

## 3) Learning-to-rank (optional)

If you log features (lexical score, vector sim, path length, timestamp gap, degree caps hit, etc.), you can train an LTR model or bandit policy to reproduce “which evidence made humans say this answer is good”. Consider after you have usage data. (Research like HYRR shows rerankers trained with hybrid signals are robust across different first-stage retrievers.) ([arXiv](https://arxiv.org/abs/2212.10528?utm_source=chatgpt.com "HYRR: Hybrid Infused Reranking for Passage Retrieval"))

---

# C) Diversity & de-dup (don’t feed your LLM 10 copies of the same thing)

## 1) MMR (Maximal Marginal Relevance)

Classic, still effective: pick the next item that best trades off **relevance vs. novelty** relative to what’s already selected. It’s a simple loop that reduces redundancy and improves coverage of distinct aspects. Great just before packaging. ([cs.cmu.edu](https://www.cs.cmu.edu/~jgc/publication/The_Use_MMR_Diversity_Based_LTMIR_1998.pdf?utm_source=chatgpt.com "The Use of MMR, Diversity-Based Reranking for Reordering Documents and ..."), [aclanthology.org](https://aclanthology.org/X98-1025/?utm_source=chatgpt.com "Summarization: (1) Using MMR for Diversity- Based Reranking and (2 ..."))

## 2) Near-duplicate control

Even after MMR, drop candidates that are almost identical spans or the _same path_ written twice. (Shingling/overlap thresholds over TextUnits; path hashing for subgraphs.) Use this as a fast filter _before_ MMR to save budget.

---

# D) Evidence packaging: what you actually hand to the LLM

Package **just enough** to answer + cite, nothing more:

## 1) For _graph_ evidence

- **Nodes** (IDs, labels, key props)
    
- **Edges** (type, props, _valid_from/valid_to_)
    
- **Paths** (ordered node/edge list, path length)
    
- Optional **community IDs** or motif tags (helps the LLM contextualize “why this path”).
    
- Keep everything **timestamped**; bias toward evidence within the user’s window.
    

## 2) For _text_ grounding (TextUnits)

- Document ID/URI, section/offsets, extracted snippet
    
- Source time (authored/ingested) + extractor version (for audit)
    
- Confidence scores (if you have them)
    

Microsoft’s GraphRAG model explicitly separates **TextUnits**, **Entities/Relationships**, and **Community Reports**, and uses these artifacts at query time—copy that pattern so your generator can cite both the **subgraph** and the **supporting text** behind each claim. ([Microsoft GitHub](https://microsoft.github.io/graphrag/index/overview/?utm_source=chatgpt.com "Overview - GraphRAG"))

**Packaging shape (example):**

```json
{
  "graph_paths": [
    {
      "nodes": [{"id":"svc:A","t":"Service"}, {"id":"chg:PR-123","t":"Change"}],
      "edges": [{"t":"AFFECTED_BY","valid_from":"2025-07-12"}],
      "why": ["shortest", "temporal_match"]
    }
  ],
  "text_spans": [
    {"doc":"incidents/123.md","offset":[412,690],"time":"2025-07-12","conf":0.86}
  ],
  "meta": {"fusion":"rrf", "reranker":"fit5", "mmr_lambda":0.3}
}
```

---

# E) A reference pipeline that’s easy to tune

1. **Gather candidates**
    
    - Run **BM25** and **vector** (and optionally _global/community_) seeders → union.
        
    - Fuse with **RRF** (or a 1-parameter convex blend if you can tune). ([Microsoft Learn](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview?utm_source=chatgpt.com "Hybrid search - Azure AI Search | Microsoft Learn"), [arXiv](https://arxiv.org/abs/2210.11934?utm_source=chatgpt.com "An Analysis of Fusion Functions for Hybrid Retrieval"))
        
2. **Expand & score**
    
    - For each seed: **k-hop** with degree/type/time caps; collect **paths**, attach **TextUnits**.
        
    - Compute structure scores (path length, motif match, temporal validity gap).
        
3. **Re-rank**
    
    - **Text**: cross-encoder/LLM rerank of (query, TextUnit) (e.g., HYRR / FiT5-style).
        
    - **Structure**: boost items that satisfy motifs/temporal rules; down-weight hubby paths. ([arXiv](https://arxiv.org/abs/2212.10528?utm_source=chatgpt.com "HYRR: Hybrid Infused Reranking for Passage Retrieval"))
        
4. **De-dup & diversify**
    
    - Drop near-dupes, then run **MMR** over the remaining set to keep diversity within your token budget. ([cs.cmu.edu](https://www.cs.cmu.edu/~jgc/publication/The_Use_MMR_Diversity_Based_LTMIR_1998.pdf?utm_source=chatgpt.com "The Use of MMR, Diversity-Based Reranking for Reordering Documents and ..."))
        
5. **Package**
    
    - Emit compact **graph paths + text spans** with timestamps and provenance, ready for grounded generation. (GraphRAG’s separation of TextUnits and Communities is a solid template.) ([Microsoft GitHub](https://microsoft.github.io/graphrag/index/overview/?utm_source=chatgpt.com "Overview - GraphRAG"))
        

---

# F) Tuning cliff-notes (what to try first)

- **Fusion**: start with **RRF** (k≈60 works well out-of-the-box in Azure); if you can label ~50 queries, try a **learned weight** for lexical vs vector and keep the winner. ([Microsoft Learn](https://learn.microsoft.com/en-us/azure/search/hybrid-search-ranking?utm_source=chatgpt.com "Hybrid search scoring (RRF) - Azure AI Search | Microsoft Learn"), [arXiv](https://arxiv.org/abs/2210.11934?utm_source=chatgpt.com "An Analysis of Fusion Functions for Hybrid Retrieval"))
    
- **Re-ranker**: if latency allows, add a cross-encoder reranker; it usually gives the biggest lift per unit effort. ([arXiv](https://arxiv.org/abs/2212.10528?utm_source=chatgpt.com "HYRR: Hybrid Infused Reranking for Passage Retrieval"))
    
- **Graph-aware boosts**: +score for shorter valid-time paths and for motif hits; cap degree to avoid hubs. (DRIFT/Global summaries can also guide which local nodes to emphasize.) ([Microsoft GitHub](https://microsoft.github.io/graphrag/query/overview/?utm_source=chatgpt.com "Overview - GraphRAG"))
    
- **Diversity**: MMR with λ≈0.2–0.4 is a good starting range to avoid redundant chunks/paths. ([cs.cmu.edu](https://www.cs.cmu.edu/~jgc/publication/The_Use_MMR_Diversity_Based_LTMIR_1998.pdf?utm_source=chatgpt.com "The Use of MMR, Diversity-Based Reranking for Reordering Documents and ..."))
    

---

If you want, share your **question archetypes**, **current retrievers**, and a rough **latency budget**. I’ll turn this into an explicit fusion/rerank config (with RRF or learned blend), an MMR step, and a concrete evidence bundle schema matched to your graph—and align it with GraphRAG’s “global/local/DRIFT” query modes so you’re not reinventing the wheel.