[[Neo4j]]
[[Wrapper]]

Put a wrapper into something you want to turn into a database cluster using the this code:

## **A GraphRAG Example With Neo4j**

[[What Is GraphRAG - Graph Database & Analytics]]

A frequent use case for GraphRAG is analyzing research information in more detail than just “chatting with your PDF.” In a vector-only semantic search approach, the data returned from the retrievers are just scored chunks of text with little or no information on how they relate to concepts from the domain or each other.

In contrast, a GraphRAG approach allows us to extract entities such as Person, Organization, Article, Paper, BiologicalProcess, Condition, Disease, Drug, Gene, Expression, Exposure, and Pathway that appear in our documents and create a rich network of information.

To demonstrate this, let’s walk through an example of constructing a knowledge graph using the open source [opens in new tabneo4j-graphrag](https://neo4j.com/blog/news/graphrag-python-package/) package. You can also use LangChain, LlamaIndex, or other [opens in new tabintegrations](https://neo4j.com/labs/genai-ecosystem/langchain/).

In this example, we use the SimpleKGPipeline, which comes with a number of defaults and executes the steps depicted below:

![The SimpleKGPipeline process.](https://dist.neo4j.com/wp-content/uploads/20241015075828/simplekgpipeline-1.png)

To run this extraction, we configure the Pipeline with the following components:

- LLM (e.g., gpt-4‌‌‌o-mini from OpenAI)
- Embedding model
- Document splitter
- Graph schema

Once configured, we can execute the pipeline on our dataset of biomedical research papers:

driver = neo4j.GraphDatabase.driver(NEO4J_URI, auth=(NEO4J_USERNAME, NEO4J_PASSWORD))nnex_llm=OpenAILLM(n    model_name=u0022gpt-4o-miniu0022,n    model_params={n        u0022response_formatu0022: {u0022typeu0022: u0022json_objectu0022},n        u0022temperatureu0022: 0n    }n)nembedder = OpenAIEmbeddings()nnnode_labels = [u0022Anatomyu0022, u0022BiologicalProcessu0022, ...]nrel_types = [u0022ACTIVATESu0022, u0022AFFECTSu0022, u0022ASSESSESu0022,...u0022TREATSu0022, u0022USED_FORu0022]nnkg_builder_pdf = SimpleKGPipeline(n    llm=ex_llm,n    driver=driver,n    text_splitter=FixedSizeSplitter(chunk_size=500, chunk_overlap=100),n    embedder=embedder,n    entities=node_labels,n    relations=rel_types,n    prompt_template=prompt_template,n    from_pdf=Truen)nnpdf_file_paths = ['biomolecules-11-00928-v2.pdf', 'GAP-between-patients-and-clinicians_2023_Best-Practice.pdf','pgpm-13-39.pdf']nnfor path in pdf_file_paths:n    graph_data = await kg_builder_pdf.run_async(file_path=path)

Copied

After storing the chunked document data and graph data in Neo4j, we can visualize it using our Query tool.

![Graph data visualization](https://dist.neo4j.com/wp-content/uploads/20241204071517/graph-data-visualization-query-tool.png)

Now, we can execute a GraphRAG retriever and compare its results with a vector RAG retriever.

This retriever first executes a vector search for the indexed text chunks and then follows non-chunk relationships up to 2 hops out, retrieving not only the directly extracted entities but also their first- and second-degree neighbors. It returns the chunk texts and entity-relationship-entity pairs as context for use in the final phases of prompt augmentation and answer generation.

from neo4j_graphrag.retrievers import VectorCypherRetrievernngraph_retriever = VectorCypherRetriever(n    driver,n    index_name=u0022text_embeddingsu0022,n    embedder=embedder,n    retrieval_query=u0022u0022u0022n//1) Go out 2-3 hops in the entity graph and get relationshipsnWITH node AS chunknMATCH (chunk)u0026lt;-[:FROM_CHUNK]-(entity)-[relList:!FROM_CHUNK]-{1,2}(nb)nUNWIND relList AS relnn//2) collect relationships and text chunksnWITH collect(DISTINCT chunk) AS chunks, collect(DISTINCT rel) AS relsnn//3) format and return contextnRETURN apoc.text.join([c in chunks | c.text], '
') + n  apoc.text.join([r in rels | n  startNode(r).name+' - '+type(r)+' '+r.details+' -u003e '+endNode(r).name],  n  '
') AS infonu0022u0022u0022n)

Copied

Next, we build the vector and GraphRAG pipelines using each retriever with a suitable LLM (here, using the better OpenAI gpt-4‌‌o) and prompt for question answering:

llm = LLM(model_name=u0022gpt-4ou0022,  model_params={u0022temperatureu0022: 0.0})nnrag_template = RagTemplate(template='''Answer the Question using the following Context. Only respond with information mentioned in the Context. Do not inject any speculative information not mentioned. nn# Question:n{query_text}n n# Context:n{context}nn# Answer:n''', expected_inputs=['query_text', 'context'])nnvector_rag  = GraphRAG(llm=llm, retriever=vector_retriever, prompt_template=rag_template)nngraph_rag = GraphRAG(llm=llm, retriever=graph_retriever, prompt_template=rag_template)nnq = u0022Can you summarize systemic lupus erythematosus (SLE)? including common effects, biomarkers, and treatments? Provide in detailed list format.u0022nnvector_rag.search(q, retriever_config={'top_k':5}).answerngraph_rag.search(q, retriever_config={'top_k':5}).answer

Copied

The comparison of answers shows that the GraphRAG response is much more comprehensive and covers more of the relevant context.

![Comparison of vector RAG answers vs. GraphRAG answers.](https://dist.neo4j.com/wp-content/uploads/20241204071538/answers-comparison.png)

For more details, see [opens in new tabGraphRAG Python Package: Accelerating GenAI With Knowledge Graphs](https://neo4j.com/blog/news/graphrag-python-package/) and check out the resources section.

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

and [[Neo4j Bloom]] to visualize everything. You can say "the cluster database is AI generated". You can usen this to map out LLM conversation that you have had like the idea [[3D LLM Graph]].