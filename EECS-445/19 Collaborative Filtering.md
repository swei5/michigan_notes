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

Then, we first initialize $\overline{v}^{(1)}, \cdots, \overline{v}^{(m)}$ to **small**, **random** values.
```shell
while not converged:
	fix v[1], ..., v[m]
	solve u[1], ..., u[n]

	fix u[1], ..., u[n]
	solve v[1], ..., v[m]
end
```

```ad-example
**Example**: Solving for optimum

![[Pasted image 20230401222207.png|500]]
![[Pasted image 20230401222226.png|500]]
```

---

### Nearest Neighbor
Using our previous example, we want to compute **similarity** between user $a$ and **ALL OTHER** users in the system.
- Find the $k$ **nearest neighbors** of user $a$ who have rated movie $i$

Let's define a few things beforehand.
$$\tilde{Y}_{a:b}=\frac{1}{|R(a,b)|}\sum\limits_{j\in R(a,b)}Y_{a_{j}}$$
where $R(a,b)$ is the set of movies rated by both users $a$ and $b$.

We can thus define **similarity**, as a measure of **correlation**, as known as Sample Pearson's correlation coefficient:
![[Pasted image 20230331012410.png|500]]

- The numerator denotes the **covariance** between $a$ and $b$
	- How much ratings vary together
- The denominator denotes the **standard deviation** of $a$ and $b$
	- How much ratings vary individually

#### Algorithm
Suppose $Y_{ai}$ is what we are trying to predict.
1. Compute the **similarity** between $a$ and the rest
2. Find the $k$ **nearest neighbors** of $a$, $k_j$, for instance, where $Y_{k_{j}i}$ is filled
3. Compute a prediction based on $Y_{k_{j}, i}$, $\forall j\in[1,k]$.

![[Pasted image 20230331143913.png|500]]

```ad-example
Say if we were to use $kNN$, where $k=1$ to predict the missing cell below.

![[Pasted image 20230331160712.png|600]]

First, we want to find the **similarity** between $a$ and $b$. Hence,
$$\begin{align}R(a,b)=\set{2,6}\end{align}$$
$$|R(a,b)|=2$$
$$\hat{Y}_{a:b}=\frac{Y_{a2}+Y_{a6}}{2}=\frac{4+5}{2}$$
$$\hat{Y}_{b:a}=\frac{Y_{b2}+Y_{b6}}{2}=\frac{5+3}{2}=4$$

from which we can compute $\text{sim}(a,b)$:
$$\text{sim}(a,b)=\frac{(Y_{a2}-\hat{Y}_{a:b})(Y_{b2}-\hat{Y}_{b:a})+\cdots}{\sqrt{((Y_{a2}-\hat{Y}_{a:b})^{2}+(Y_{b2}-\hat{Y}_{b:a}))\cdots}}=1.$$

Then, we can move on to calculating the perdiction. We will need $\overline{Y_{a}}$ and $\overline{Y_{b}}$ first:
$$\overline{Y_{a}}=\frac{5+4}{2}=4.5$$
$$\overline{Y_{b}}=\frac{5+1+3}{3}=3$$

Thus,
$$\hat{Y}_{a3}=\overline{Y_{a}}+\frac{1}{\text{sim}(a,b)}\cdot(\text{sim}(a,b)(Y_{bi}-\overline{Y}_{b}))=4.5+1(1-3)=2.5.$$
```

#### Choosing $k$
Heuristics to choose our hyperparameters include:
- Consider **ALL neighbors**
- Neighbors with **low correlation**
- Variable $k$
	- Varying neighborhoods are selected **for each** item based on a similarity **threshold**
- Offline analysis : values in the 20–50 range area reasonable starting point in many domains

---

### Discriminative and Generative Models
- Discriminative models
	- Internal structure of the classes is not captured
	- E.g. **classification**
- Generative models
	- Describes internal structure of the data
	- Better understand where data *came from*
	- Can also be used in classification