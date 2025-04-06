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
$$C=\frac{\text{Number of closed triplets}}{n(\text{number of all triplets})}$$

---
## Question 2
### (a)
### (b)
### (c)

---
## Question 3
### (a)
### (b)
### (c)