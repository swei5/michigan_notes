[[2023-03-20]] #UnsupervisedLearning #Clustering #Spectral

### Spectral Clustering

```ad-important
**Definition 17.1**: Spectral Clustering

Spectral Clustering reformulates data clustering problem as **graph partitioning** problem.
- Doesn't assume globular-shaped clusters

Broadly, we
- First convert data into a weighted graph
- Next, partition graph so that each component has a **weaker** across- partition connection and **stronger** within-partition connection; ensure similar sized partitions

Measured by similarity metric - $\text{sim}(A,B)$, where $s \in [0,1]$, with $0$ being dissimilar and $1$ being most similar
- $\text{sim}(A,B)=\text{sim}(B,A)$
```

#### Data Transformation
Given $S_{n}=\set{\overline{x}^{(i)}}_{i=1}^{n}$, we first compute **similarity** between each pair of datapoints by
$$w_{ij}=\exp\left(-\frac{||\overline{x}^{(i)}-\overline{x}^{(j)}||^2}{2\sigma^{2}}\right)$$

From which we obtain an **adjacency matrix** $W=\{w_{ij}\}$, $i=1,\cdots,n$, $j=1,\cdots,n$, where $W$ is symmetric and $w_{ij}>0$.

```ad-important
**Definition 17.2**: Cost of a cut

$$\text{cut}(A, \overline{A})=\sum\limits_{i\in A, j\in \overline{A}} w_{ij}$$

In plain words, this is the **sum** of weights of edges going between $A$ and $\overline{A}$, where $\overline{A}$ is the complement to $A$.
```

```ad-example
Example of cost of a cut:

![[Pasted image 20230325211114.png|400]]
```

Now, our goal is to find a **minimum cost** RatioCut given such graph representation:
$$\text{RatioCut}(A_1,\cdots,A_{k})=\frac{1}{2} \sum\limits_{i=1}^{k} \frac{\text{cut}(A_{i},\overline{A}_{i})}{|A_{i}|}$$

Let's consider the cut using a partition indicator function $f\in\mathbb{R}^n$, where for cut $A$ and $\overline{A}$:
$$\begin{equation}
  f_{i} =
    \begin{cases}
      \sqrt{|\overline{A}|/|A|} \text{ if }v_{i}\in A\\
      -\sqrt{|A|/|\overline{A}|} \text{ if }v_{i}\in \overline{A}\\
    \end{cases} \end{equation}$$
And our goal is to show that
$$\overline{f}^{T}L\overline{f}\propto \text{RatioCut}(A,\overline{A})$$ and$$\sum\limits_{i}f_{i}=0$$and$$||\overline{f}||^2=n.$$
It turns out that $$\overline{f}^{T}L\overline{f}=R=\frac{1}{2}\sum\limits_{i,j}w_{ij}(\overline{f}_{i}-\overline{f}_{j})^2$$
where $R$ is the **cost of partitioning**. Thus, we have transformed our problem to $$f^{*}=\text{argmin}_{f\in\mathbb{R}^{n}}\overline{f}^{T}L\overline{f}$$
We perform the minimization by clustering the rows of the matrix, which is what [[#Build Spectral Clustering for $k$ Partitions]] describes.

This is hard to compute. We want to take advantage of the **graph Laplacian matrix**. First, we will define a **degree matrix** $D$.

```ad-important
**Definition 17.3**: Degree Matrix
With $d_{ii}=\sum\limits_{j=1}^{n} w_{ij}$, and $d_{ij}=0 \ \ \forall i\ne j$.

$$
D = \begin{bmatrix} 
    d_{11} & \dots & 0 \\
    \vdots & \ddots & \vdots\\
    0 &   \dots     & d_{nn} 
    \end{bmatrix}
    = \begin{bmatrix} 
    \sum\limits_{j}^{n}w_{1j} & \dots & 0 \\
    \vdots & \ddots & \vdots\\
    0 &   \dots     & \sum\limits_{j}^{n}w_{nj} 
    \end{bmatrix}
    $$

Note that $L$ is symmetric and $L$ has $n$ non-negative real-valued eigenvalues
$$0\le \lambda_{1}\le \cdots \le \lambda_{n}$$
```

The **multiplicity** of the **eigenvalue** $0$ is the number of **connected components** in the graph.

From that, we obtain our **graph Laplacian**:
$$L=D-W$$

---

### Build Spectral Clustering for $k$ Partitions
Let's begin with our input data, as well as number of clusters $k$.

1. Build adjacency matrix $W$
2. Compute graph Laplacian $L=D-W$
3. Compute (eigenvector, eigenvalue) pairs of $L$
4. Build matrix with the first $k$ eigenvectors (corresponding to the $k$ **smallest** **eigenvalues**) as columns interpret **rows** as new **data** points
5. Apply $k$ -means

After this, we get our cluster assignment.

```ad-example
After we grabbed $k=3$ eigenvalue, eigenvector pairs from $L$, we have the following

![[Pasted image 20230325220424.png|400]]

We select the first $k=3$ eigenvectors that correspond to the eigenvalues $0,0,0$. This gives us a $5\times 3$ matrix: we intepret each row as a data point to run $k$-means on.
```
