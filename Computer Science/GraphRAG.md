#computer_science #AI #machine_learning #LLM  #data_science #GenAI #database #graphs 

[[Types of GraphRAG]]
[[Complete GraphRAG Workflow]]
[[GraphRAG - Drift]]
[[GraphRAG - Local]]
[[GraphRAG - Global]]

https://neo4j.com/blog/genai/what-is-graphrag/

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

## **Types of GraphRAG Retrievers**

The actual graph retrieval depends on the use case and domain. Different types of retrievers can be combined, and their results ranked, combined, or sequenced. In an agentic setup, retrievers can become tools that the LLM selects and runs iteratively, passing parameters and results until the necessary information to answer the question is collected.

Examples of GraphRAG retriever types include:

- **Vector (Embedding), Fulltext, Spatial, or other Search Indexes**: Using index searches with information from the user question to determine starting points in the graph for further exploration.
- **Neighborhood Traversal**: Access direct or indirect neighbors of a node to put a piece of information into context.
- **Path Traversals**: Find paths between starting entities, expand relationships to their neighborhood, and retrieve additional related documents, claims, and other entities.
- **Global Queries:** Using pre-computed, cross-topic summarization and other global representations of insights to answer general questions (see Microsoft’s GraphRAG with Query Focused Summarization).
- **[opens in new tabQuery Templates](https://graphrag.com/reference/graphrag/cypher-templates/)**: Use case-specific queries for categories of questions are provided by a domain expert, can have the same starting points but explore different sub-graphs, and can be selected by categorizing questions.
- **[opens in new tabDynamic Cypher Generation](https://graphrag.com/reference/graphrag/dynamic-cypher-generation/)** **(****[opens in new tabText2Cypher](https://graphrag.com/reference/graphrag/text2cypher)****)**: A (fine-tuned) LLM generates a Cypher query from the user question and the graph schema description to answer specific and structural questions.
- **Agentic Traversal**: Using different retrievers, an LLM selects and executes them in a planned sequence to collect all information to answer the question.
- **Graph Embedding Retrievers**: Using embeddings to represent the “essence” of a node’s neighborhood and allow fuzzy topological search by matching candidate embeddings.

You can find more examples in the GraphRAG Pattern Catalog on [opens in new tabgraphrag.com](http://graphrag.com/).
