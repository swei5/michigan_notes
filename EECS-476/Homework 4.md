**EECS 476 Homework 4**
Vincent Wei ([vswei@umich.edu](mailto:vswei@umich.edu))
April 15, 2025

## Question 1
### (a)
A.
### (b)(i)
| Vertex | Degree |
| ------ | ------ |
| $A$    | $3$    |
| $B$    | $3$    |
| $C$    | $6$    |
| $D$    | $2$    |
| $E$    | $4$    |
| $F$    | $4$    |
| $G$    | $3$    |
| $H$    | $2$    |
| $I$    | $1$    |
Average degree: $\frac{3+3+6+2+4+4+3+2+1}{9}=3.111$.

### (b)(ii)
$$D=\frac{2|E|}{|V|(|V|-1)}=\frac{28}{9\cdot8}=0.389$$
### (b)(iii)
We can compute a distance matrix (assume equal length for all edges).

| Vertex | $A$ | $B$ | $C$ | $D$ | $E$ | $F$ | $G$ | $H$ | $I$ |
| ------ | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| $A$    | 0   | 1   | 1   | 2   | 2   | 1   | 2   | 3   | 4   |
| $B$    |     | 0   | 1   | 2   | 2   | 1   | 2   | 3   | 4   |
| $C$    |     |     | 0   | 1   | 1   | 1   | 1   | 2   | 3   |
| $D$    |     |     |     | 0   | 1   | 2   | 2   | 2   | 3   |
| $E$    |     |     |     |     | 0   | 2   | 1   | 1   | 2   |
| $F$    |     |     |     |     |     | 0   | 1   | 3   | 4   |
| $G$    |     |     |     |     |     |     | 0   | 2   | 3   |
| $H$    |     |     |     |     |     |     |     | 0   | 1   |
| $I$    |     |     |     |     |     |     |     |     | 0   |
The longest distance of the shortest paths between any two vertices and hence the diameter is $4$.

### (b)(iv)
Node $C$ has the highest value in betweenness. It is on the shortest path between $(A, E)$, $(A,D)$, $(A,H)$, $(A,I)$, $(B,D)$, $(B,E)$, $(B,G)$, $(B,H)$, $(B,I)$, $(D,F)$, $(D,G)$, $(E,F)$ and it can be concluded that no other nodes appear as frequent as node $A$ on the shortest paths between all pairs of nodes in the graph.

### (b)(v)
Node $C$ has the highest value in closeness. The sum of its shortest path to all the other nodes is the shortest among all nodes - it has a distance of $1$ to $A, B, E, F, G$, $2$ to $H$ and $3$ to $I$. All the other nodes have a sum that is notably higher.

### (b)(vi)
We can count the number of closed triplets (triangles) manually and the number of triplets for each vertex $i$ as $C(\text{deg}(v_{i}),2)$.
$$C=\frac{\text{number of closed triplets}}{n(\text{number of all triplets})}=\frac{7}{47}=0.149$$

---
## Question 2
### (a)
A, B.
### (b)(i)
$$M=\begin{bmatrix}0 & 0 & 1 & 0  \\ \frac{1}{2} & 0 & 0 & 0  \\  \frac{1}{2} & 1 & 0 & 1 \\ 0 & 0 & 0 & 0\end{bmatrix}$$

### (b)(ii)
$$\begin{align}
\mathbf{r}^{1}&=\left(\beta M+(1-\beta) \left[\frac{1}{n}\right]_{4 \times 4}\right)\mathbf{r}^{0}\\
&= \begin{bmatrix}0.0375 & 0.0375 & 0.8875 & 0.0375\\
0.4625 & 0.0375 & 0.0375 & 0.0375\\
0.4625 & 0.8875 & 0.0375 & 0.8875\\
0.0375 & 0.0375 & 0.0375 & 0.0375\end{bmatrix} \begin{bmatrix}0.25 \\
0.25 \\
0.25 \\
0.25\end{bmatrix}\\
&= \begin{bmatrix}0.25 \\
0.144\\
0.569\\
0.038\end{bmatrix}
\end{align}$$
### (b)(iii)
Similar to (ii), we can compute $\mathbf{r}^{2}$ as $$\begin{align}
\mathbf{r}^{2}&=\left(\beta M+(1-\beta) \left[\frac{1}{n}\right]_{4 \times 4}\right)\mathbf{r}^{1} \\
&= \begin{bmatrix}0.0375 & 0.0375 & 0.8875 & 0.0375\\
0.4625 & 0.0375 & 0.0375 & 0.0375\\
0.4625 & 0.8875 & 0.0375 & 0.8875\\
0.0375 & 0.0375 & 0.0375 & 0.0375\end{bmatrix} \begin{bmatrix}0.25 \\
0.144 \\
0.569 \\
0.038\end{bmatrix}\\
&= \begin{bmatrix}0.521\\
0.144\\
0.298\\
0.038\end{bmatrix}
\end{align}$$
### (b)(iv)
If $D \to C$ is reversed, then $D$ becomes a dangling node. This violates the assumption of Column-stochasticity - that columns within an adjacency matrix $M$ should sum up to $1$ ($D$ will have sum of $0$ if no teleports were implemented). 

### (b)(v)
I ran the following code to determine the expected PageRank vector:

```python
import numpy as np

M = np.array([
    [0,   0,   1,   0],
    [0.5, 0,   0,   0],
    [0.5, 1,   0,   1],
    [0,   0,   0,   0]
])

beta = 0.85
n = M.shape[0]
teleport = np.ones((n, 1)) / n

r = np.ones((n, 1)) / n

eps = 1e-8
delta = 1
while delta > eps:
    r_new = beta * M @ r + (1 - beta) * teleport
    delta = np.linalg.norm(r_new - r, 1)
    r = r_new

print("PageRank vector:\n", r.flatten())
```

The resulting vector is $$\mathbf{r}= \begin{bmatrix}0.373  \\ 0.196 \\ 0.394 \\ 0.038\end{bmatrix}$$
### (c)
First, we initialize the adjacency matrix $A$: $$\begin{bmatrix}0 & 1 & 1 & 0 \\ 0 & 0 & 1 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0\end{bmatrix}$$ as well as the authority and hub vectors: $$a=h=\begin{bmatrix} 0.5 \\ 0.5 \\ 0.5\\ 0.5 \end{bmatrix}$$
First iteration:
$$\begin{align}
a = A^{T}h &=\begin{bmatrix}0 & 0 & 1 & 0 \\ 1 & 0 & 0 & 0 \\ 1 & 1 & 0 & 1 \\ 0 & 0 & 0 & 0\end{bmatrix}\begin{bmatrix} 0.5 \\ 0.5 \\ 0.5 \\ 0.5 \end{bmatrix}\\
&= \begin{bmatrix} 0.5\\ 0.5 \\ 1.5 \\ 0
\end{bmatrix}\\
h=Aa &= \begin{bmatrix}0 & 1 & 1 & 0 \\ 0 & 0 & 1 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0\end{bmatrix}\begin{bmatrix} 0.5 \\ 0.5 \\ 0.5 \\ 0.5 \end{bmatrix}\\
&= \begin{bmatrix}1 \\ 0.5 \\ 0.5 \\ 0.5
\end{bmatrix}
\end{align}$$
Normalizing gives $$a= \frac{1}{\sqrt{3}} \begin{bmatrix}0.5\\ 0.5 \\ 1.5 \\ 0\end{bmatrix}$$ and $$h= \frac{1}{\sqrt{1.75}} \begin{bmatrix}1 \\ 0.5 \\ 0.5 \\ 0.5
\end{bmatrix}$$
We repeat the process for the second iteration: $$\begin{align}
a= A^Th &=\begin{bmatrix}0 & 0 & 1 & 0 \\ 1 & 0 & 0 & 0 \\ 1 & 1 & 0 & 1 \\ 0 & 0 & 0 & 0\end{bmatrix}\frac{1}{\sqrt{1.75}} \begin{bmatrix}1 \\ 0.5 \\ 0.5 \\ 0.5
\end{bmatrix}\\
&= \begin{bmatrix} 0.378\\ 0.756 \\ 1.512 \\ 0
\end{bmatrix}\\
h = Aa &= \begin{bmatrix}0 & 1 & 1 & 0 \\ 0 & 0 & 1 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0\end{bmatrix}\frac{1}{\sqrt{3}} \begin{bmatrix}0.5\\ 0.5 \\ 1.5 \\ 0\end{bmatrix}\\
&= \begin{bmatrix}1.155 \\ 0.866 \\ 0.289 \\ 0.866
\end{bmatrix}
\end{align}$$ After applying normalization, we have $$a= \frac{1}{\sqrt{3}} \begin{bmatrix} 0.378\\ 0.756 \\ 1.512 \\ 0
\end{bmatrix}$$ and $$h=\frac{1}{\sqrt{2.917}} \begin{bmatrix}1.155 \\ 0.866 \\ 0.289 \\ 0.866
\end{bmatrix}$$

---
## Question 3
### (a)
A, C.
### (b)
$A$: $0$
- Explanation: the structure overgeneralizes and is not better off than a randomized structure
$B$: $0.39$
- Explanation: this visually matches the left and right square structures and exhibits strong intra-cluster and minimal inter-cluster edges
$C$: $0.19$
- Explanation: this is somewhat between $A$ and $B$ in that there is some community structure based on the partitions but not optimal
$D$: $-0.13$
- Explanation: Each node is a community itself (no community structure)

### (c)(i)
Because there is $1$ edge between all pairs of communities after the first pass, the weight of each edge in graph $H$ is exactly $2$.

### (c)(ii)
The modularity is given by $$Q= \frac{1}{2m} \sum\limits_{i,j} \left[A_{ij}- \frac{k_{i}k_{j}}{2m}\right] \delta(c_{i},c_{j})$$ Note that $m=8$ because there are $4$ edges, each of weight $2$.

Since we're calculating modularity after the first pass, each node in $H$ is its own community. For any pair of nodes $i$ and $j$ that are connected:
- $A_{ij}=2$
- $k_{i}=k_{j}=4$
- $\delta(c_{i},c_{j})=0$

And for any node with itself (same community),
- $A_{ii}=0$
- $\delta(c_{i},c_{i})=1$

We now have enough information to compute $Q$: $$\begin{align}
Q&= \frac{1}{16} \sum\limits_{i,i} \left[- \frac{k_{i}k_{i}}{2m}\right]\\
&=  \frac{1}{16} (4)  \frac{-16}{16}\\
&= - \frac{1}{4}
\end{align}$$

### (c)(iii)
The modularity gain when moving a node $i$ to community $C$ is given by: $$\Delta Q = \left[\frac{\Sigma_{in} + k_{i,in}}{2m} - \left(\frac{\Sigma_{tot} + k_i}{2m}\right)^2\right] - \left[\frac{\Sigma_{in}}{2m} - \left(\frac{\Sigma_{tot}}{2m}\right)^2 - \left(\frac{k_i}{2m}\right)^2\right]$$ 
In our graph $H$: 
-  $m = 8$ (as shown previously)
- $k_i = 4$ 
And for any community $C$ consisting of a single node $j$:
-  $\Sigma_{in} = 0$ (no internal edges in a single-node community)
-  $\Sigma_{tot} = 4$ (total weight of edges connected to node $j$)
- $k_{i,in} = 2$ (weight of edge between node $i$ and node $j$) 

Substituting in the previous equation shows:
$$\begin{align} \Delta Q &= \left[\frac{0 + 2}{2 \cdot 8} - \left(\frac{4 + 4}{2 \cdot 8}\right)^2\right] - \left[\frac{0}{2 \cdot 8} - \left(\frac{4}{2 \cdot 8}\right)^2 - \left(\frac{4}{2 \cdot8}\right)^2\right] \\ &= \left[\frac{2}{16} - \left(\frac{8}{16}\right)^2\right] - \left[0 - \left(\frac{4}{16}\right)^2 - \left(\frac{4}{16}\right)^2\right] \\ &= \left[\frac{2}{16} - \frac{64}{256}\right] - \left[0 - \frac{16}{256} - \frac{16}{256}\right] \\ &= \left[\frac{2}{16} - \frac{1}{4}\right] - \left[0 - \frac{32}{256}\right] \\ &= \left[\frac{1}{8} - \frac{1}{4}\right] - \left[-\frac{1}{8}\right] \\ &= -\frac{1}{8} + \frac{1}{8} \\ &= 0 \end{align}$$

---
## Question 4
### (a)(i)
### (a)(ii)
### (a)(iii)
### (b)(i)
### (b)(ii)

---
## Question 5
### (a)
Since $h_{A}^{(0)}=1$ and $A$ has $1$ neighbor, $h_{A}^{(1)}=1+1=2$.

### (b)
Similar to (a), we can work out $h_{B}^{1}=1+1=2$.

The neighbor of $B$ has $3$ neighbors (including $B$). Hence, $$h_{neighbor}^{1} = h_{neighbor}^{0} + \sum_{v \in \mathcal{N}_{neighbor}} h_{v}^{0}=1+3=4$$
This gives $$h_{B}^{2}=h_{B}^{1} +h_{neighbor}^{1}=2+4=6$$
### (c)
First we need to compute $h^{2}_{A}$. Since $A$ 's neighbor has just the same number of neighbors as $B$ 's neighbor, we can conclude that $h_{A}^{2}=6$ as well. Therefor the second layer fails to distinguish and we need to look at the third layer.

Without actual calculation, we can tell that $A$ and $B$ would have different embeddings at layer $k=3$. This is because both ($2$) of the neighbors of $A$ 's neighbor are connected to $2$ other neighbors, but only one of the $2$ neighbors of $B$ 's neighbor is connected to $2$ other neighbors, and the other one is only connected to $1$. This implies $h^{2}_{A's neighbor}$ will be larger than $h^{2}_{B'sneighbor}$ and therefore causing $h^{3}_{A}$ and $h^{3}_{B}$ to differ.