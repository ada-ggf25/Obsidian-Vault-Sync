#computer_science #AI #machine_learning #LLM  #data_science #GenAI
 
 GraphRAG is a powerful retrieval mechanism that improves [GenAI](https://neo4j.com/blog/genai/what-is-generative-ai/) applications by taking advantage of the rich context in graph data structures.

Enterprise GenAI systems face a critical challenge: the need for trustworthy and reliable results. Pure large language model (LLM)-based solutions often fall short in this regard. These models are trained to prioritize helpfulness over factuality, and their pre-training data usually lacks crucial recent and relevant information. Consequently, they are prone to generating hallucinations of facts and explanations, which is particularly damaging in high-value business domains and use cases.

To address these issues, [opens in new tabRetrieval-Augmented Generation](https://neo4j.com/blog/genai/what-is-retrieval-augmented-generation-rag/) (RAG) architectures have emerged as a solution. RAG improves the reliability of GenAI components by ensuring that LLM answers are based only on accurate information from existing knowledge sources.

Basic RAG systems rely solely on semantic search in vector databases to retrieve and rank sets of isolated text fragments. While this approach can surface some relevant information, it fails to capture the context connecting these pieces. For this reason, basic RAG systems are ill-equipped to answer complex, multi-hop questions.

This is where GraphRAG comes in. It uses [opens in new tabknowledge graphs](https://neo4j.com/blog/genai/what-is-knowledge-graph/) to represent and connect information to capture not only more data points but also their relationships. Thus, graph-based retrievers can provide more accurate and relevant results by uncovering hidden connections that aren’t often obvious but are crucial for correlating information.

In this blog post, we’ll dive into how GraphRAG works, explore its advantages over other RAG architectures in improving answer quality and explainability, and demonstrate its practical application using a Neo4j example.

## **Introduction to Retrieval-Augmented Generation (RAG)**

Before diving into the specifics of GraphRAG, it’s essential to understand the basic concepts of RAG. Let’s take a closer look at the three key phases of RAG:

**Retrieval:** In this phase, the RAG system retrieves relevant information from external data sources, such as documents or databases, based on the user’s query. The retrieval process can use different techniques to identify the most pertinent data, such as similarity searches or database queries. The results are then ranked and scored based on their relevance to the query.

**Augmentation:** During the augmentation phase, the retrieved information is combined with the original user question, along with any additional instructions or context. This augmented prompt provides a richer context for the language model to generate a response. The goal is to force the model to only use this relevant information to produce an accurate and useful output.

**Generation:** In the final phase, the augmented prompt is processed by an LLM, which generates an answer in a requested format using only the provided context rather than relying on its pre-trained knowledge. The response can also link source information and additional metadata.

By augmenting the language model with external knowledge and using the model’s natural language understanding capabilities to retrieve and process this information, RAG systems can produce more accurate and informative responses compared to standalone language models that rely solely on their pre-trained knowledge.

![The retrieval-augmented generation process](https://dist.neo4j.com/wp-content/uploads/20240227120536/rag-process.png)

## **Limitations of Vector-Only RAG**

Many baseline RAG systems rely solely on [opens in new tabvector search](https://neo4j.com/labs/genai-ecosystem/vector-search/?utm_source=GSearch&utm_medium=PaidSearch&utm_campaign=Evergreen&utm_content=AMS-Search-SEMCE-DSA-None-SEM-SEM-NonABM&utm_term=&utm_adgroup=DSA-GenAI&gad_source=1&gclid=CjwKCAiA9bq6BhAKEiwAH6bqoB6nBE_VccroKOKJ4tRqwf5ot1JJcPBM6BHiU_DbTD8IuvzIMlSfJBoC9O8QAvD_BwE) over text embeddings (numerical vector representations) for information retrieval. To accurately capture the cohesive semantic meaning of a piece of text, the source documents are often chunked into smaller fragments, which are then embedded, indexed, and stored for retrieval.

However, this approach has its limitations. By relying solely on vector search, the response’s content is confined to the text fragments in the retrieved chunks. This can lead to incomplete or fragmented answers.

For example, if a user asks a question about a specific product feature, a vector-only RAG system might retrieve chunks that mention the product but fail to include relevant information from other parts of the documentation that provide a more comprehensive answer.

Moreover, due to the black-box nature of vector representations and vector search, such methods cannot explain the sources of the gathered information. This means that users and developers have limited visibility into why certain chunks were retrieved and how they contribute to the generated response. This lack of explainability can be a significant drawback, particularly in domains where transparency and accountability are crucial, such as healthcare or finance.

To address these shortcomings, new techniques are emerging to improve different phases of the RAG process (retrieval, augmentation, and generation).

Since the information provided to the LLM is crucial to answer quality, improving the retrieval mechanism often has the most significant impact. GraphRAG is a common approach to enhance retrieval by incorporating structured domain knowledge stored in a knowledge graph. By tapping into the rich connections and semantic relationships in a knowledge graph, GraphRAG aims to overcome the limitations of vector-only RAG and provide more accurate and explainable responses.

## **Knowledge Graph for Data Representation**

A [opens in new tabknowledge graph](https://neo4j.com/blog/genai/what-is-knowledge-graph/) model is especially suitable for representing structured and unstructured data with connected elements. Unlike traditional databases, they don’t require a rigid schema but are more flexible in the data model. The graph model allows efficient storage, management, querying, and processing of the richness of real-world information. In a RAG system, the knowledge graph serves as the flexible memory companion to the language skills of LLMs, such as summarization, translation, and extraction.

![Knowledge graph components](https://dist.neo4j.com/wp-content/uploads/20241205002358/knowledge-graph-5-e1733387331979.png)

In a knowledge graph, facts and entities are represented as _nodes_ with attributes connected with typed _relationships,_ which also carry attributes for qualification. This graph model can scale from a simple family tree to the complete digital twin of a company encompassing employees, customers, processes, products, partnerships, and resources, with millions or billions of connections.

Graph structures can originate from various sources, from a structured business domain, (hierarchical) document representations, and signals computed by graph algorithms.

[](https://neo4j.com/whitepapers/gartner-how-to-build-knowledge-graphs-that-enable-ai-driven-enterprise-applications/)

### Read a Complimentary Gartner® Report on Knowledge Graphs for AI

## **Graph Querying for GraphRAG Retrievers**

Graphs can be navigated (traversed) by following simple patterns like `(node:Type)-[relationship:TYPE]->(node:Type)` or more complex variants expressed in Graph query languages like Cypher or GQL. Pattern matching results in paths whose nodes, relationships, and attributes can be filtered, aggregated, and sorted like in other query languages like SQL. Here is an example of a graph query that returns neighborhood information from a vector embedding search:

CALL db.index.vector.queryNodes(docs, 5, $embedding) yield node as doc, scorenRETURN score, doc, COLLECT { nMATCH path = (doc)-[rel]-(neighbor)nRETURN pathn} as pathsnORDER BY score DESC LIMIT 10

## **How GraphRAG Improves Retrieval**

A GraphRAG retrieval can find _starting points_ in this network of data via vector, fulltext, spatial, or other searches and then _follow relevant relationships_ to gather additional information to satisfy the user queries. The context of the user and task is considered to increase relevance. All captured nodes, relationships and their attributes can be filtered and ranked before being returned as context in the augmentation phase.

This approach offers several advantages over vector-only RAG systems:

1. By navigating the graph structure and following relevant relationships, GraphRAG can retrieve information that may not be directly mentioned in the initial set of retrieved chunks, providing a more comprehensive and contextually relevant response.
2. The ability to filter and rank the retrieved information based on the user’s context and task allows GraphRAG to prioritize the most pertinent information, improving the overall quality of the generated response.
3. GraphRAG enables better explainability by capturing the relationships between the retrieved information, making it easier to trace the sources and reasoning behind the generated response.
4. By using the knowledge graph’s ability to integrate structured and unstructured data, as well as computed signals, GraphRAG can provide more informed and nuanced responses that draw from a wider range of information sources.

These improvements in the retrieval phase contribute to GraphRAG’s ability to generate more accurate, relevant, and traceable responses compared to vector-only RAG systems.

## **Types of GraphRAG Retrievers**

The actual graph retrieval depends on the use case and domain. Different types of retrievers can be combined, and their results ranked, combined, or sequenced. In an agentic setup, retrievers can become tools that the LLM selects and runs iteratively, passing parameters and results until the necessary information to answer the question is collected.

Examples of GraphRAG retriever types include:

- **Vector (Embedding), Fulltext, Spatial, or other Search Indexes**: Using index searches with information from the user question to determine starting points in the graph for further exploration.
- **Neighborhood Traversal**: Access direct or indirect neighbors of a node to put a piece of information into context.
- **Path Traversals**: Find paths between starting entities, expand relationships to their neighborhood, and retrieve additional related documents, claims, and other entities.
- **Global Queries:** Using pre-computed, cross-topic summarization and other global representations of insights to answer general questions (see Microsoft’s GraphRAG with Query Focused Summarization).
- **[opens in new tabQuery Templates](https://graphrag.com/reference/graphrag/cypher-templates/)**: Use case-specific queries for categories of questions are provided by a domain expert, can have the same starting points but explore different sub-graphs, and can be selected by categorizing questions.
- **[opens in new tabDynamic Cypher Generation](https://graphrag.com/reference/graphrag/dynamic-cypher-generation/)** **(****[opens in new tabText2Cypher](https://graphrag.com/reference/graphrag/text2cypher)****)**: A (fine-tuned) LLM generates a Cypher query from the user question and the graph schema description to answer specific and structural questions.
- **Agentic Traversal**: Using different retrievers, an LLM selects and executes them in a planned sequence to collect all information to answer the question.
- **Graph Embedding Retrievers**: Using embeddings to represent the “essence” of a node’s neighborhood and allow fuzzy topological search by matching candidate embeddings.

You can find more examples in the GraphRAG Pattern Catalog on [opens in new tabgraphrag.com](http://graphrag.com/).

## **Knowledge Graph Construction**

For GraphRAG to work well, we need to ensure that our data has a shape that accurately represents the highly relevant, connected pieces of information. To create this knowledge graph, we need to follow two steps, which can be repeated for refinement:

1. Model the relevant nodes and relationships to represent our domain data.
2. Import, create, or compute the graph structures to fit this graph model.

![Build a knowledge graph for AI use cases.](https://dist.neo4j.com/wp-content/uploads/20241204071429/build-knowledge-graph.png)

We can combine different sources of data:

- Import existing structured data from databases, files or APIs.
- Turn unstructured data (text, audio, video) into a graph representation of document structures/hierarchies and add vector embeddings and full-text indexes for chunks.
- Construct or connect structured entities (with optional embeddings) and their relationships from textual information.
- Enhance existing graphs with additional computation or algorithms, such as topic-clustering summaries (like in Microsoft Query Focused Summarization), similarity relationships, and personalized page rank (PPR) scores.

These graph models and sources are also described in more detail in the GraphRAG pattern catalog.

## **A GraphRAG Example With Neo4j**

A frequent use case for GraphRAG is analyzing research information in more detail than just “chatting with your PDF.” In a vector-only semantic search approach, the data returned from the retrievers are just scored chunks of text with little or no information on how they relate to concepts from the domain or each other.

In contrast, a GraphRAG approach allows us to extract entities such as Person, Organization, Article, Paper, BiologicalProcess, Condition, Disease, Drug, Gene, Expression, Exposure, and Pathway that appear in our documents and create a rich network of information.

To demonstrate this, let’s walk through an example of constructing a knowledge graph using the open source [opens in new tabneo4j-graphrag](https://neo4j.com/blog/news/graphrag-python-package/) package. You can also use LangChain, LlamaIndex, or other [opens in new tabintegrations](https://neo4j.com/labs/genai-ecosystem/langchain/).

In this example, we use the SimpleKGPipeline, which comes with a number of defaults and executes the steps depicted below:

![The SimpleKGPipeline process.](https://dist.neo4j.com/wp-content/uploads/20241015075828/simplekgpipeline-1.png)

To run this extraction, we configure the Pipeline with the following components:

- LLM (e.g., gpt-4‌‌‌o-mini from OpenAI)
- Embedding model
- Document splitter
- Graph schema

Once configured, we can execute the pipeline on our dataset of biomedical research papers:

driver = neo4j.GraphDatabase.driver(NEO4J_URI, auth=(NEO4J_USERNAME, NEO4J_PASSWORD))nnex_llm=OpenAILLM(n    model_name=u0022gpt-4o-miniu0022,n    model_params={n        u0022response_formatu0022: {u0022typeu0022: u0022json_objectu0022},n        u0022temperatureu0022: 0n    }n)nembedder = OpenAIEmbeddings()nnnode_labels = [u0022Anatomyu0022, u0022BiologicalProcessu0022, ...]nrel_types = [u0022ACTIVATESu0022, u0022AFFECTSu0022, u0022ASSESSESu0022,...u0022TREATSu0022, u0022USED_FORu0022]nnkg_builder_pdf = SimpleKGPipeline(n    llm=ex_llm,n    driver=driver,n    text_splitter=FixedSizeSplitter(chunk_size=500, chunk_overlap=100),n    embedder=embedder,n    entities=node_labels,n    relations=rel_types,n    prompt_template=prompt_template,n    from_pdf=Truen)nnpdf_file_paths = ['biomolecules-11-00928-v2.pdf', 'GAP-between-patients-and-clinicians_2023_Best-Practice.pdf','pgpm-13-39.pdf']nnfor path in pdf_file_paths:n    graph_data = await kg_builder_pdf.run_async(file_path=path)

After storing the chunked document data and graph data in Neo4j, we can visualize it using our Query tool.

![Graph data visualization](https://dist.neo4j.com/wp-content/uploads/20241204071517/graph-data-visualization-query-tool.png)

Now, we can execute a GraphRAG retriever and compare its results with a vector RAG retriever.

This retriever first executes a vector search for the indexed text chunks and then follows non-chunk relationships up to 2 hops out, retrieving not only the directly extracted entities but also their first- and second-degree neighbors. It returns the chunk texts and entity-relationship-entity pairs as context for use in the final phases of prompt augmentation and answer generation.

from neo4j_graphrag.retrievers import VectorCypherRetrievernngraph_retriever = VectorCypherRetriever(n    driver,n    index_name=u0022text_embeddingsu0022,n    embedder=embedder,n    retrieval_query=u0022u0022u0022n//1) Go out 2-3 hops in the entity graph and get relationshipsnWITH node AS chunknMATCH (chunk)u0026lt;-[:FROM_CHUNK]-(entity)-[relList:!FROM_CHUNK]-{1,2}(nb)nUNWIND relList AS relnn//2) collect relationships and text chunksnWITH collect(DISTINCT chunk) AS chunks, collect(DISTINCT rel) AS relsnn//3) format and return contextnRETURN apoc.text.join([c in chunks | c.text], '
') + n  apoc.text.join([r in rels | n  startNode(r).name+' - '+type(r)+' '+r.details+' -u003e '+endNode(r).name],  n  '
') AS infonu0022u0022u0022n)

Next, we build the vector and GraphRAG pipelines using each retriever with a suitable LLM (here, using the better OpenAI gpt-4‌‌o) and prompt for question answering:

llm = LLM(model_name=u0022gpt-4ou0022,  model_params={u0022temperatureu0022: 0.0})nnrag_template = RagTemplate(template='''Answer the Question using the following Context. Only respond with information mentioned in the Context. Do not inject any speculative information not mentioned. nn# Question:n{query_text}n n# Context:n{context}nn# Answer:n''', expected_inputs=['query_text', 'context'])nnvector_rag  = GraphRAG(llm=llm, retriever=vector_retriever, prompt_template=rag_template)nngraph_rag = GraphRAG(llm=llm, retriever=graph_retriever, prompt_template=rag_template)nnq = u0022Can you summarize systemic lupus erythematosus (SLE)? including common effects, biomarkers, and treatments? Provide in detailed list format.u0022nnvector_rag.search(q, retriever_config={'top_k':5}).answerngraph_rag.search(q, retriever_config={'top_k':5}).answer

The comparison of answers shows that the GraphRAG response is much more comprehensive and covers more of the relevant context.

![Comparison of vector RAG answers vs. GraphRAG answers.](https://dist.neo4j.com/wp-content/uploads/20241204071538/answers-comparison.png)

For more details, see [opens in new tabGraphRAG Python Package: Accelerating GenAI With Knowledge Graphs](https://neo4j.com/blog/news/graphrag-python-package/) and check out the resources section.

## **Common GraphRAG Use Cases**

GraphRAG is used in applications and domains that require a higher level of trust, as their outputs are used for critical business decision-making. Some examples include:

- **Legal and Compliance**: Reviewing and analyzing contracts, cases, laws, and regulations.
- **Investment Research**: Investigating organizations, people, competitors, markets, and trends.
- **Biotech**: Accessing knowledge graphs for drug discovery and repurposing, clinical trials, and research.
- **Business Process Support**: Integrating various business data sources into a cohesive view of an organization.
- **Supply Chain**: Conducting investigations for risk assessment, compliance, and sustainability of products and production processes.
- **Fraud Detection**: Identifying and preventing money laundering (AML), insurance fraud, and other fraudulent activities.
- **Investigative Journalism**: Uncovering connections and patterns in large datasets for news stories and investigations.
- **Natural Language Search and Chatbots**: Democratizing access to pre-existing knowledge bases through user-friendly interfaces.

## **GraphRAG: Enabling Enterprise-Grade AI Apps**

RAG architectures are currently the most effective way to provide reliable content for GenAI business applications by using data from trusted data sources. GraphRAG takes this a step further, improving upon basic vector-based RAG in both quality and explainability.

By considering more relevant context and using a variety of retrievers that navigate document, domain, and computed graph structures, GraphRAG delivers more accurate, trustworthy, and traceable results. The combination of knowledge graphs, with their rich representation of real-world information, and LLMs, with their advanced language skills, creates a robust and reliable solution for enterprise use cases.

As more organizations adopt GenAI, GraphRAG will be essential in ensuring the accuracy, reliability, and transparency of these systems, paving the way for better decision-making and improved business outcomes.

### Essential GraphRAG

Unlock the full potential of RAG with knowledge graphs. Get the definitive guide from Manning, free for a limited time.

## **Additional Resources**

If you want to learn more about GraphRAG, check out these resources:

**Overviews**

- [opens in new tabGraphRAG Manifesto](https://neo4j.com/blog/genai/graphrag-manifesto/)
- [opens in new tabWhat is a Knowledge Graph](https://neo4j.com/blog/genai/what-is-knowledge-graph/)
- [opens in new tabGenerative AI with Neo4j](https://neo4j.com/generativeai)

**Technical**

- [opens in new tabGraphRAG Pattern Catalog](https://graphrag.com/)
- [opens in new tabOnline Neo4j LLM Knowledge Graph Builder (using LangChain)](https://neo4j.com/labs/genai-ecosystem/llm-graph-builder/)
- [opens in new tabNeo4j GraphRAG Python Package](https://neo4j.com/blog/news/graphrag-python-package/)

**Courses**

- [opens in new tabDeepLearning.AI Knowledge Graphs for RAG course](https://www.deeplearning.ai/short-courses/knowledge-graphs-rag/)
- [opens in new tabFree GraphAcademy GenAI courses](https://graphacademy.neo4j.com/categories/llms/)