[[2023-03-27]] #UnsupervisedLearning #Filtering

### Motivation
Assume we have a utility matrix of size $m \times n$ for $n$ customers and $m$ movies. We have some partial knowledge about *some* customers' preferences of *some* movies only, how do we solve for the missing ratings?

```ad-question
Given $Y$ with empty cells, how can we construct **low rank** $\hat{Y}$ to approximate $Y$ and use $\hat{Y}$ to predict missing ratings?
```

---

### Matrix Factorization

```ad-important
**Theorem 19.1**: Low-rank Factorization

Let $\hat{Y} \in \mathbb{R}^{n\times m}$ and $\text{rank}(\hat{Y})=r$. Then, there is $U\in \mathbb{R}^{n \times r}$ and $V^{T}\in \mathbb{R}^{r \times m}$ such that $\hat{Y}=UV^T$.

$r$ is the number of linearly independent columns in $\hat{Y}$.
```

```ad-example
In our previous movie example, $U$ contains relevant features of the **user**, and $V$ contains relevant features of the **movie**.

![[Pasted image 20230330100835.png|500]]
```

#### Algorithm
Let $\overline{u}^{(a)} \in \mathbb{R}^d$ be the $a$ th row of $U$
$$U=\begin{bmatrix}\overline{u}^{(1)}, \cdots,\overline{u}^{(n)}\end{bmatrix}^T$$
Let $\overline{v}^{(i)}$ be the $i$ th row of $V$
$$V^T=\begin{bmatrix}\overline{v}^{(1)}, \cdots,\overline{v}^{(m)}\end{bmatrix}$$

Then, by rules of matrix multiplication,
$$\hat{Y}_{ai}=[UV^{T}]_{ai}=\overline{u}^{(a)}\cdot \overline{v}^{(i)}$$
From which we can derive our objective function:
$$\begin{align}J(U,V)&=\frac{1}{2}\sum\limits_{a,i \in D}(Y_{ai}-[UV^T]_{ai})^{2}+\frac{\lambda}{2}\sum\limits_{a,k}(U_{ak})^2\\&=\frac{1}{2}\sum\limits_{a,i \in D}(Y_{ai}-[UV^T]_{ai})^{2}+\frac{\lambda}{2}\sum\limits_{a}||\overline{u}^{(a)}||^{2}+\frac{\lambda}{2}\sum\limits_{i}||\overline{v}^{(i)}||^{2}\end{align}$$
Note that this is [[8 Regression, Regularization#^e607e4|ridge regression]]!

---

### Nearest Neighbor
Using our previous example, we want to compute **similarity** between user $a$ and **ALL OTHER** users in the system.
- Find the $k$ **nearest neighbors** of user $a$ who have rated movie $i$

Let's define a few things beforehand.
$$\tilde{Y}_{a:b}=\frac{1}{|R(a,b)|}\sum\limits_{j\in R(a,b)}Y_{a_{j}}$$
where $R(a,b)$ is the set of movies rated by both users $a$ and $b$.

We can thus define **similarity**, as a measure of **correlation**, as known as Sample Pearson's correlation coefficient:
![[Pasted image 20230331012410.png|500]]

- The nominator denotes the **covariance** between $a$ and $b$
	- How much ratings vary together
- The denominator denotes the **standard deviation** of $a$ and $b$
	- How much ratings vary individually

#### Algorithm


