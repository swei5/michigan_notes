[[2024-09-12]] #Portfolio #Valuation #Risks 

```ad-important
**Definition 6.1**: Feasible Set

The **collection** of all portfolios that can be constructed by investing in given assets is called the **feasible** (or attainable) **set**.
```

![[Pasted image 20240912131943.png|400]]
The parametrized curve (with parameters $w_{1},w_2$) has to **pass through** the portfolio with only $S_1$ or $S_2$, corresponding to the scenario where $w_{1}=1$ and $w_{2}=1$, regardless of how $\rho_{12}$ changes.

### Returns of Several Securities
Now suppose that the market has $n$ different stocks, then a portfolio can be described by weights (in these stocks): $$w_{i}=\frac{x_{i}S_{i}(0)}{V(0)}$$
For $i=1,\dots, n$. Note that $$V(0)=\sum\limits_{i=1}^{n}x_{i}S_{i}(0)$$
Let $K_i$ be the return of stock $i$, then we have $$K_{V}=\sum\limits_{i=1}^{n}w_{i}K_i$$
With the restriction that $\sum_{i=1}^{n}w_{i}=1$.

```ad-important
**Definition 6.2**: Properties in Matrix Notation

We define weight **row vector** ($1 \times n$) $$w:=[w_{1},\dots,w_{n}]$$ and $$u:=[1,\dots,1]$$
Thus $$uw^T=1$$

We define the **mean vector** ($1 \times n$) $$m:=[\mu_{1},\dots, \mu_{n}]$$ where $\mu_{i}=\mathbb{E}(K_{i})$

We define the **covariance matrix** ($n\times n$) $$C=(c_{ij})_{n\times n}$$ where $c_{ij} := \text{Cov}(K_{i},K_{j})$.
```

Note that since we define the weight and mean vectors as row vectors, the notation is opposite to what we've seen in other linear algebra materials, where both $w,u$ are treated as column vectors.

Note that $C$ is symmetric ($C=C^T$) and positive definite ($XCX^{T}\ge 0$) for any real vector $X^{T}\in  \mathbb{R}$ ($1 \times n$ row vector). 
- The diagonal elements are the variances

We assume that $C$ is invertible, i.e. there exists $C^{-1}$.

```ad-important
**Definition 6.3**: Expected Return and Variance 

The expected return $\mu_{V}=\mathbb{E}(K_{V})$ and variance $\sigma_{V}^{2}=\text{Var}(K_{V})$ of a portfolio with weights $w$ are given by $$\begin{align}\mu_{V}&=\mathbb{E}(K_{V}) \\ &= \mathbb{E}\left(\sum\limits_{i=1}^{n} w_{i}K_{i}\right) \\&= \sum\limits_{i=1}^{n} w_{i}\mu_{i} = mw^{T}  \end{align}$$ 
$$\begin{align}\sigma_{V}^{2}&=\text{Var}(K_{V}) \\ &= \text{Var}\left(\sum\limits_{i=1}^{n} w_{i}K_{i}\right) \\ &= \text{Cov}\left(\sum\limits_{i=1}^{n} w_{i}K_{i}, \sum\limits_{j=1}^{n} w_{j}K_{j}\right) \\ &= \sum\limits_{i,j=1}^{n} w_{i}w_{j}c_{ij} \\ &= wCw^{T}  \end{align}$$
```

```ad-note
Given a weight vector $w$ of a portfolio, we can calculate the pair $\mu_V$ and $\sigma_V$ by the last proposition, as both are parametrized by $w$.

The collection of all such points is called the “Area of the Portfolio” (or **feasible region**, or **attainable sets**), we shadow out this region and its boundary is called **Markowitz bullet**.
- The area doesn't span the entire $\sigma, \mu$ plane since $uw^T=1$
- In the two-stock case, the area is just a **simple curve**. i.e. all portfolios lie on the curve, not what's inside
```

---
### Minimum Variance Line
We see that the minimal risk is attained at the vertex (point) of the shadowed area, which is exactly located on the Markowitz bullet. There must be a portfolio corresponding to the vertex. We call it the **minimal variance portfolio**, and denote its weight by $w^\star$.

```ad-important
**Definition 6.4**: Minimum Variance Portfolio

The portfolio with the smallest variance in the attainable set is called the **minimum variance portfolio** (MVP).

The weights of the minimum variance portfolio are given by $$w_{MVP}=\frac{uC^{-1}}{uC^{-1}u^{T}}$$
```

We can show that this holds true for a two-security example, drawing conclusion from previous lecture:  ![[5 Portfolio Management I#^037b18]]
We have that $$\begin{align} w_{MVP}&=\frac{uC^{-1}}{uC^{-1}u^{T}} \\ &= \frac{ \frac{1}{\det(C)} u\begin{bmatrix} \sigma_{2}^{2} & -\sigma_{12} \\ -\sigma_{12} & \sigma_{1}^{2} \end{bmatrix}}{\frac{1}{\det(C)} u\begin{bmatrix} \sigma_{2}^{2} & -\sigma_{12} \\ -\sigma_{12} & \sigma_{1}^{2} \end{bmatrix} u^{T}} \\ &= \begin{bmatrix} \frac{\sigma_{2}^{2}-\sigma_{12}}{\sigma_{2}^{2}+\sigma_{1}^{2}-2\sigma_{12}} &  \frac{\sigma_{1}^{2}-\sigma_{12}}{\sigma_{2}^{2}+\sigma_{1}^{2}-2\sigma_{12}} \end{bmatrix} \end{align}$$
[See more for the algebra involved here](https://www.cs.rochester.edu/u/brown/Crypto/assts/projects/adj.html).

```ad-note
**Proof of Definition 6.4**

We want to solve the problem $$\min\limits_{w}wCw^{T} \text{	 subject to 	} uw^T=1$$

Since we have a minimization problem with constraints, we will use the Lagrange multipliers method. To this end, define $$\mathcal{L}(w,\lambda):= wCw^{T} - \lambda (uw^{T}-1)$$

Then, for $\nabla := (\frac{\partial}{\partial w_{1}}, \dots, \frac{\partial}{\partial w_{n}})$, we get $$\nabla \mathcal{L}=0 \iff 2wC-\lambda u =0 \iff w = \frac{\lambda}{2}C^{-1}$$
Hence $$\begin{align}uw^{T}=1&=u\left(\frac{1}{2} \lambda u C^{-1}\right)^{T} \\ &= \frac{1}{2} \lambda u C^{-1}u^{T}\\ & \implies \lambda = \frac{2}{uC^{-1}u^{T}} \\ & \implies w_{MVP}= \frac{1}{2} \lambda u C^{-1} = \frac{uC^{-1}}{uC^{-1}u^{T}} \end{align} $$
```

![[Pasted image 20240912140518.png|300]]

The bold lines are called the **Markowitz bullet**; it was named after Harry Markowitz in conjunction with its shape (“bullet”).  

An important feature of the Markowitz bullet is that each point on it has the **smallest risk among all portfolios** with the same expected return as the point. This is why the Markowitz bullet is also called the **minimum variance line**.

```ad-important
**Definition 6.5**: Minimum Variance Line

The set (parameterized on the positive values of the expected return) of the portfolios with **minimum risk for a given level of expected return** is called the **Minimum Variance Line** (MVL).
```

