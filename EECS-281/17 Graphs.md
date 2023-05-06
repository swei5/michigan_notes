[[2023-05-06]]

### Graphs

```ad-important
**Definition 17.1**: Graphs

  
$G=(V,E)$ is a set of vertices $V=\{v_1,v_2,\cdots\}$ together with a set of edges $E=\{e_1,e_2,\cdots\}$ that connects pairs of vertices.
```

Graphs **WITHOUT** *parallel edges* and **WITHOUT** *self-loops* are called **simple graphs**.
- Graph algorithms introduced in the course are under the assumption that a graph is **simple**
	- $n(E)<n(V)^{2}$ - this sets an upper limit on $E$ such that complexity cannot be infinite
	- 

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
