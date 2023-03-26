[[2023-03-18]] #UnsupervisedLearning #Clustering

### Motivation
- Labelling is labor-intensive
	- Crowd-sourcing
	- Large datasets
- Labels can be noisy
- Require good datasets

What can we do with **unlabeled data**?
- Visualize ($\dim$ less than 3)
	- Find a low dimensional embedding, then visualize
- Learn a **representation** of the data
- Uncover useful **structure**
- Identify groups/**clusters** of similar data

Applications include
- Finding similar homes for sales
- Grouping patients by symptoms
- Group search results
- Group emails
- Image processing - regions of image segmentation

---

### Clustering
In essence, we want to achieve the following
- Points that are **close** to each other should be in the **SAME** cluster
- Points that are further **apart** should be in **different** clusters

Let our input be $S=\set{\overline{x}^{(i)}}_{i=1}^{n}$ such that $\overline{x}^{(i)}\in \mathbb{R}^{d}$. And we intend to output a set of cluster assignment $c_{1}, \cdots, c_{n}$, with each $c_{i}\in\set{1,\cdots, k}$.

![[Pasted image 20230318212123.png|300]]

Clusters can be described by their corresponding set of **examples** or a **representative point**.
- An average, for instance

#### $k$ -Means Clustering
A solid and simply specified algorithm (like Perceptron) and a good starting point.

```ad-important
**Definition 16.1**: $k$-Means Clustering

To partition data into $k$ clusters (defined by $k$ “means”) such that the **sum** of squares of Euclidean distances of each point to its cluster’s mean is minimized.
```

Given the means that are corresponding to each cluster is $\overline{\mu}_{(1)},\cdots,\overline{\mu}_{(k)}$, our goal is to minimize the **objective function**
$$\sum\limits_{i=1}^{n} ||\overline{x}^{(i)}-\overline{\mu}^{(c_{i})}||^2$$

##### Algorithm

Before, initialize $k$ means. How?
- Solve an **NP-hard problem**
- **Randomly** pick amongst given data - works sometimes
- $k$ -means++ - works the **best**

![[Pasted image 20230318222107.png|400]]

```ad-note
- $j\in \set{1,\cdots,k}$ for the number of clusters
- $[[\star]]$ is an indicater function telling whether if a given datapoint $\overline{x}^{(i)}$ is in the $j$th cluster
- $\overline{\mu}^{(j)}$, in simple terms, really is taking the **average** of all points in cluster $j$.
```

##### $k$ -means++
Picking points w.p. proportional to distance from **already selected** means.

![[Pasted image 20230318222803.png|400]]

It has an **approximation guarantee**, which states that the **expected value** of the objective returned by $k$ -means++ is never more than $O (\log K)$ from **optimal** and can be as close as $O (1)$ from optimal. Even in the former case, with $2K$ random restarts, one restart will be $O (1)$ from optimal (with high probability).

---

#### Choosing $k$
A simple and handy method is the **elbow method**. Illustrated by the figure below.

![[Pasted image 20230318223312.png|300]]
The objective function dips up until a **particular point**, after which the decrease levels off. In this case, this progression of dip tells us that $k=4$ may be our optimal value to split on.

---

#### $k$ -mean Convergence
First off, let's define our **objective function**.

```ad-important
**Definition 16.2**: Objective function for $k$-mean Clustering

$$J(\overline{c}, M)=\sum\limits_{i=1}^{n} ||\overline{x}^{(i)}-\overline{\mu}^{(c_{i})}||^{2}$$

where $\overline{c}=[c_{1}, \cdots, c_{n}]^T$, where $c_{i}$ is $x^{(i)}$'s cluster and $M=[\overline{\mu}^{(1)},\cdots,\overline{\mu}^{(k)}]^T$, where $\overline{\mu}^{(j)}$ is the mean of $\overline{x}^{(i)}$'s cluster.
```

Now we claim that:
1. $k$ -means performs **coordinate descent** on the objective function
2. $k$ -means is guaranteed to **converge**

We thus want to prove that
- $J$ monotonically decrease
	- Then, $J$ must converge
		- Then, $k$ -mean is guaranteed to converge

However, $J$ is **NOT convex**, and that could put us in **local minima**:

```ad-example
What we want:

![[Pasted image 20230325205020.png|200]]

What we get:

![[Pasted image 20230325205034.png|200]]
```

Thus, to find our **global optima**, we can either
- Vary initialization of means and pick clustering with lowest $J$ ($k$ -means++)
- Or, **spectral clustering**