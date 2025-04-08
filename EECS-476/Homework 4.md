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
The modularity is given by $$Q= \frac{1}{2m} \sum\limits_{i,j} \left[A_{ij}- \frac{k_{i}k_{j}}{2m}\right] \delta(c_{i},c_{j})$$ Since we're calculating modularity after the first pass, each node in $H$ is its own community. For any pair of nodes $i$ and $j$ that are connected:
- $A_{ij}=2$
- $k_{i}=k_{j}=4$
- $\delta(c_{i},c_{j})=0$



### (c)(iii)

---
### Question 5
