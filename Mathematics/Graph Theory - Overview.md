#math #computer_science #database #data_science #graphs

This deep dive explores the mathematical foundations of graph theory, from its fundamental definitions to advanced topics spanning connectivity, planarity, coloring, matchings, spectral properties, random models, and algorithms ([Wikipedia](https://en.wikipedia.org/wiki/Graph_theory?utm_source=chatgpt.com "Graph theory"), [mathbooks.unl.edu](https://mathbooks.unl.edu/Contemporary/sec-graph-intro.html?utm_source=chatgpt.com "15.1 Introduction to Graph Theory")). We begin with basic definitions of graphs, subgraphs, and isomorphisms, and then study connectivity concepts such as Eulerian trails and Hamiltonian paths ([Wikipedia](https://en.wikipedia.org/wiki/Graph_theory?utm_source=chatgpt.com "Graph theory")). Next, we examine planarity and graph drawing, highlighted by Kuratowski’s forbidden‐minor characterization and Euler’s formula for planar graphs ([Wikipedia](https://en.wikipedia.org/wiki/Kuratowski%27s_theorem?utm_source=chatgpt.com "Kuratowski's theorem"), [discrete.openmathbooks.org](https://discrete.openmathbooks.org/more/mdm/sec_planar.html?utm_source=chatgpt.com "Planar Graphs and Euler's Formula")). We then delve into coloring problems—including vertex and edge coloring theorems like the Four Color theorem and Vizing’s theorem—before covering matchings, network flows, and extremal results such as Turán’s theorem ([Wikipedia](https://en.wikipedia.org/wiki/Four_color_theorem?utm_source=chatgpt.com "Four color theorem")). Spectral graph theory links graph structure to the eigenvalues of adjacency and Laplacian matrices, while random‐graph models like Erdős–Rényi reveal probabilistic thresholds for connectivity and phase transitions ([Wikipedia](https://en.wikipedia.org/wiki/Laplacian_matrix?utm_source=chatgpt.com "Laplacian matrix")). Finally, we discuss algorithmic aspects, from polynomial‐time shortest‐path algorithms (Dijkstra’s, Bellman–Ford) to NP‐completeness results (Hamiltonian cycle, graph coloring) and the nuanced complexity of the graph isomorphism problem ([Wikipedia](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm?utm_source=chatgpt.com "Dijkstra's algorithm")).

## 1. Basic Definitions and Concepts

### 1.1 Graphs and Terminology

A **graph** G=(V,E)G=(V,E) consists of a set of **vertices** VV and a set of **edges** EE, where each edge connects a pair of vertices ([Wikipedia](https://en.wikipedia.org/wiki/Graph_theory?utm_source=chatgpt.com "Graph theory")). Graphs may be **simple** (no loops or multiple edges), **directed** (edges have orientation), **weighted**, or **multigraphs** depending on additional structure ([mathbooks.unl.edu](https://mathbooks.unl.edu/Contemporary/sec-graph-intro.html?utm_source=chatgpt.com "15.1 Introduction to Graph Theory")).

### 1.2 Subgraphs, Induced Subgraphs, and Minors

A **subgraph** is obtained by deleting vertices or edges; an **induced subgraph** deletes vertices and all incident edges ([Wikipedia](https://en.wikipedia.org/wiki/Graph_theory?utm_source=chatgpt.com "Graph theory")). A **minor** of a graph is formed by deleting edges/vertices and contracting edges; minors characterize many deep theorems like Robertson–Seymour ([Wikipedia](https://en.wikipedia.org/wiki/Subgraph_isomorphism_problem?utm_source=chatgpt.com "Subgraph isomorphism problem")).

### 1.3 Graph Isomorphism

Two graphs GG and HH are **isomorphic** if there exists a bijection between their vertex sets preserving adjacency ([Wikipedia](https://en.wikipedia.org/wiki/Graph_isomorphism?utm_source=chatgpt.com "Graph isomorphism")). The **graph isomorphism problem**—deciding whether two graphs are isomorphic—is in NP but neither known to be in P nor NP‐complete, and Babai’s quasi‐polynomial‐time algorithm represents a landmark advance ([Wikipedia](https://en.wikipedia.org/wiki/Graph_isomorphism_problem?utm_source=chatgpt.com "Graph isomorphism problem"), [WIRED](https://www.wired.com/2015/12/landmark-algorithm-breaks-30-year-impasse?utm_source=chatgpt.com "Landmark Algorithm Breaks 30-Year Impasse")).

## 2. Connectivity and Traversability

### 2.1 Connectivity

A graph is **connected** if there is a path between every pair of vertices; **vertex-connectivity** and **edge-connectivity** measure resilience to vertex or edge removal, respectively ([Wikipedia](https://en.wikipedia.org/wiki/Connectivity_%28graph_theory%29?utm_source=chatgpt.com "Connectivity (graph theory)")).

### 2.2 Eulerian Paths and Circuits

An **Eulerian trail** visits every edge exactly once; an **Eulerian circuit** starts and ends at the same vertex. A finite graph has an Eulerian circuit if and only if it is connected and every vertex has even degree, and it has an Eulerian trail (but not circuit) if and only if exactly two vertices have odd degree ([Wikipedia](https://en.wikipedia.org/wiki/Eulerian_path?utm_source=chatgpt.com "Eulerian path"), [math.hawaii.edu](https://math.hawaii.edu/~les/m100/lecture7?utm_source=chatgpt.com "Section 5. Euler's Theorems.")).

### 2.3 Hamiltonian Paths and Cycles

A **Hamiltonian path** visits each vertex exactly once, and a **Hamiltonian cycle** returns to its start. Determining existence is NP‐complete in general, though special classes admit polynomial‐time tests ([Wikipedia](https://en.wikipedia.org/wiki/Hamiltonian_path?utm_source=chatgpt.com "Hamiltonian path")).

## 3. Planar Graphs and Graph Drawing

### 3.1 Planar Graphs

A graph is **planar** if it can be drawn without edge crossings. Equivalently, it contains no subdivision of K5K_5 or K3,3K_{3,3} (Kuratowski’s theorem) ([Wikipedia](https://en.wikipedia.org/wiki/Kuratowski%27s_theorem?utm_source=chatgpt.com "Kuratowski's theorem")).

### 3.2 Euler’s Formula

For a connected planar graph with vv vertices, ee edges, and ff faces,

v−e+f=2,v - e + f = 2,

a relation that underpins many planar‐graph proofs (e.g., sparsity bounds e≤3v−6e \le 3v-6) ([Wikipedia](https://en.wikipedia.org/wiki/Planar_graph?utm_source=chatgpt.com "Planar graph")).

## 4. Graph Coloring

### 4.1 Vertex Coloring and the Four Color Theorem

A **proper vertex coloring** assigns colors to vertices so adjacent vertices differ. The **chromatic number** χ(G)\chi(G) is the fewest colors needed. The Four Color theorem states χ(G)≤4\chi(G)\le4 for every planar graph; first proved with computer assistance in 1976 ([Wikipedia](https://en.wikipedia.org/wiki/Four_color_theorem?utm_source=chatgpt.com "Four color theorem")).

### 4.2 Edge Coloring and Vizing’s Theorem

An **edge coloring** colors edges so incident edges differ. Vizing’s theorem asserts that every simple graph of maximum degree Δ\Delta has chromatic index χ′(G)∈{Δ,Δ+1}\chi'(G)\in\{\Delta,\Delta+1\} ([Wikipedia](https://en.wikipedia.org/wiki/Vizing%27s_theorem?utm_source=chatgpt.com "Vizing's theorem")).

### 4.3 Brooks’ Theorem

Brooks’ theorem provides χ(G)≤Δ\chi(G)\le\Delta for any connected graph unless it is a complete graph or an odd cycle, which require Δ+1\Delta+1 colors ([Wikipedia](https://en.wikipedia.org/wiki/Brooks%27_theorem?utm_source=chatgpt.com "Brooks' theorem")).

## 5. Matchings and Network Flows

### 5.1 Matchings and Hall’s Marriage Theorem

A **matching** is a set of edges with no shared vertices. In bipartite graphs, Hall’s theorem gives a necessary and sufficient condition for a perfect matching: every subset of one part has at least as many neighbors in the other ([Wikipedia](https://en.wikipedia.org/wiki/Hall%27s_marriage_theorem?utm_source=chatgpt.com "Hall's marriage theorem")).

### 5.2 Tutte’s Theorem on Perfect Matchings

Tutte’s theorem characterizes graphs with perfect matchings via a parity‐based condition on odd components of vertex‐deleted subgraphs ([Wikipedia](https://en.wikipedia.org/wiki/Tutte%27s_theorem?utm_source=chatgpt.com "Tutte's theorem")).

### 5.3 Max-Flow Min-Cut Theorem

In any network, the maximum value of an ss-tt flow equals the minimum capacity of an ss-tt cut; efficient algorithms include Ford–Fulkerson and Edmonds–Karp ([Wikipedia](https://en.wikipedia.org/wiki/Cut_%28graph_theory%29?utm_source=chatgpt.com "Cut (graph theory) - WikipediaMax-flow min-cut theorem - Wikipedia")).

## 6. Spectral Graph Theory

### 6.1 Adjacency and Laplacian Matrices

The **adjacency matrix** AA and the (combinatorial) **Laplacian** L=D−AL=D-A encode graph structure; eigenvalues of LL reflect connectivity (e.g., the second smallest “Fiedler value”) and those of AA relate to walks and expansion ([math.uchicago.edu](https://math.uchicago.edu/~may/REU2012/REUPapers/JiangJ.pdf?utm_source=chatgpt.com "an introduction to spectral graph theory"), [Wikipedia](https://en.wikipedia.org/wiki/Laplacian_matrix?utm_source=chatgpt.com "Laplacian matrix")).

### 6.2 Cheeger’s Inequality and Applications

Cheeger’s inequality links the conductance (expansion) of a graph to its spectral gap, with applications in clustering and sparsest‐cut approximations ([people.orie.cornell.edu](https://people.orie.cornell.edu/dpw/orie6334/Fall2016/lecture7.pdf?utm_source=chatgpt.com "Lecture 7 1 Normalized Adjacency and Laplacian Matrices")).

## 7. Extremal Graph Theory

### 7.1 Turán’s Theorem

Turán’s theorem bounds the maximum number of edges in an nn-vertex graph without a Kr+1K_{r+1} subgraph; the extremal case is the complete rr-partite Turán graph T(n,r)T(n,r) ([Wikipedia](https://en.wikipedia.org/wiki/Tur%C3%A1n%27s_theorem?utm_source=chatgpt.com "Turán's theorem")).

### 7.2 Erdős–Stone Theorem

Generalizing Turán’s result, the Erdős–Stone theorem gives asymptotically precise bounds on ex(n;H)ex(n;H) for arbitrary forbidden subgraphs HH ([Wikipedia](https://en.wikipedia.org/wiki/Erd%C5%91s%E2%80%93Stone_theorem?utm_source=chatgpt.com "Erdős–Stone theorem")).

## 8. Random Graphs and Probabilistic Methods

### 8.1 Erdős–Rényi Models

The G(n,p)G(n,p) and G(n,M)G(n,M) models connect pairs of nn vertices with independent probability pp or exactly MM edges. Key thresholds (e.g., for connectivity at p≈ln⁡nnp\approx\frac{\ln n}{n}) describe phase transitions ([Wikipedia](https://en.wikipedia.org/wiki/Erd%C5%91s%E2%80%93R%C3%A9nyi_model?utm_source=chatgpt.com "Erdős–Rényi model")).

### 8.2 Giant Component and Phase Transitions

When np>1np>1, a unique “giant” component emerges of size Θ(n)\Theta(n); below this threshold, all components are O(ln⁡n)O(\ln n) ([Wikipedia](https://en.wikipedia.org/wiki/Erd%C5%91s%E2%80%93R%C3%A9nyi_model?utm_source=chatgpt.com "Erdős–Rényi model")).

## 9. Algorithmic Aspects and Complexity

### 9.1 Shortest-Path Algorithms

- **Dijkstra’s algorithm** finds single‐source shortest paths in O(m+nlog⁡n)O(m + n\log n) time for nonnegative weights ([Wikipedia](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm?utm_source=chatgpt.com "Dijkstra's algorithm")).
    
- **Bellman–Ford algorithm** handles negative weights in O(nm)O(nm) time ([Wikipedia](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm?utm_source=chatgpt.com "Bellman–Ford algorithm")).
    

### 9.2 NP-Completeness in Graph Theory

- Hamiltonian cycle and proper kk-coloring are NP‐complete in general ([Wikipedia](https://en.wikipedia.org/wiki/Hamiltonian_path?utm_source=chatgpt.com "Hamiltonian path")).
    
- Graph isomorphism lies in NP∩co‐AM and is GI‐intermediate under current assumptions ([Wikipedia](https://en.wikipedia.org/wiki/Graph_isomorphism_problem?utm_source=chatgpt.com "Graph isomorphism problem")).
    

---

This overview outlines the rigorous mathematics and diverse applications of graph theory, providing a foundation for further study in combinatorial, algebraic, geometric, probabilistic, and computational aspects of networks.

___

