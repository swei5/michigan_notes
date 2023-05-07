[[2023-05-06]]

### Graphs

```ad-important
**Definition 17.1**: Graphs

  
$G=(V,E)$ is a set of vertices $V=\{v_1,v_2,\cdots\}$ together with a set of edges $E=\{e_1,e_2,\cdots\}$ that connects pairs of vertices.
```

Graphs **WITHOUT** *parallel edges* and **WITHOUT** *self-loops* are called **simple graphs**.
- Graph algorithms introduced in the course are under the assumption that a graph is **simple**
	- $n(E)<n(V)^{2}$ - this sets an upper limit on $E$ such that complexity cannot be infinite

```ad-important
**Definition 17.2**: Graph, Additional Definitions
- **Simple path**
	- Sequence of edges leading from one vertex to another with NO vertex appearing twice
- **Connected graph**
	- A simple path exists, for all pair of vertices
- **Cycle**
	- A simple path, except that first and final nodes are the same
- **Directed graph**
	- Edges have direction - **ordered pairs** of nodes
		- $e_{u,v}$ means there is an edge **from** $u$ **to** $v$
- **Undirected graph**
	- Edges have **NO** direction - **unordered pairs** of nodes
		- $e_{u,v}$ means there is an edge **between** $u$ **and** $v$
```

#### Types of Graphs
- **Complete graph**
	- Contains all possible edges: $|E|=|V|\cdot \frac{|V-1|}{2} \approx |V|^{2}$
- **Dense graph**
	- A graph with many edges: $|E|\approx |V|^2$
		- Better represented as **adjmat**
- **Sparse graph**
	- A graph with few edges: $|E| \ll |V|^{2}$ or $|E| \approx |V|$
		- Better represented as **adjlist**

##### Adjacency Matrix
A $|V| \times |V|$ matrix representing what is connected to what.
- Note that undirected adjmat only needs $\frac{V^{2}}{2}$ space since the matrix is **diagonal**
	- However, better use a full matrix to avoid segfault

In an unweighted graph,
- `0`: no edge
- `1`: edge

In a weighted graph, intuitively,
- `∞`: no edge. In C++, we use `numeric_limits<double>::infinity()` in the `<limits>` library, or `INT_MAX`
- `val`: value of the edge

##### Adjacency List
A similar idea, but in form of a list.

```ad-note
If we assume random distribution of edges, then
- There are $\frac{E}{V}$ edges on each vertex list, on average
- Accessing each vertex list is $O(1)$ if we have an effective hash table
- Finding an edge on the list is $O(\frac{E}{V})$
	- This is linear search, as in separate chaining

This means the average cost for each individual vertex (*finding its edge with an arbitrary vertex*) is $O(1+ \frac{E}{V})$
```
