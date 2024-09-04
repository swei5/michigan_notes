[[2024-09-03]] #Regression 

### Recap
Recall some of the ground rules we have for **[[2 Multiple Regression#^859af4|weaker linear regression]]** ![[2 Multiple Regression#^0125a8]] ![[2 Multiple Regression#^fda1c8]]
![[2 Multiple Regression#^167b66]]

In matrix notation, our weaker linear model takes the form
$$y=X\beta + \epsilon$$
With $\mathbb{E}(\epsilon)=0$ (Assumption 1), $\text{Var}(\epsilon)=\sigma_{\epsilon}^{2}I_{n \times n}$ (Assumption 2). Here, we consider $X$ (design matrix) to be **fixed**, $\beta$ to be **fixed** but **UNKNOWN**, and $\epsilon$ **random** - the only factor that contributes to the randomness of our model.

Note that $$\mathbb{E}(y)=\mathbb{E}(X\beta)+\mathbb{\epsilon}=\mathbb{E}(X\beta)$$
And also $$\text{Var}(y)=\text{Var}(X\beta)+\text{Var}(\epsilon)=\sigma_{\epsilon}^{2}I_{n \times n}$$
Since we keep $X$, $\beta$ fixed and hence their product has variance $\mathbf{0}$.

---
### Projection
We introduce an additional quantity called the **Hat matrix**, which will be useful in what follows. 

```ad-important
**Definition 3.1**: Hat Matrix

We define that $$\hat{y}=X\hat{\beta}=X(X^{T}X)^{-1}X^{T}y=HY$$
where $$H=X(X^{T}X)^{-1}X^{T}$$

Intuitively, it is called the hat matrix because it *puts on the hats* on $y$.

Using the hat matrix, we can rewrite the fitted values and residuals of our model
$$\hat{y}=Hy$$
$$e=y-\hat{y}=(I-H)y$$
where $I$ is the $n \times n$ identity matrix.
```
\
We would like to establish a few useful properties for the hat matrix.

```ad-important
**Definition 3.2**: Orthogonal Projection Matrix 

In general, a matrix $P$ is an orthogonal projection matrix if:
- $P^{T}=P$ - $P$ is **symmetric**
- $PP=P$ - $P$ is **idempotent**
```

```ad-note
$H$ is an orthogonal projection matrix. Proof:

$$\begin{align} H^{T}&=(X(X^{T}X)^{-1}X^{T})^{T} \\
&= X (X(X^{T}X)^{-1})^{T} \\
&= X(X^{T}X)^{-1})^{T}X^{T} \\
&= X(X^{T}X)^{-1}X^{T} = H
\end{align}$$

And, $$
\begin{align}
	HH&= X(X^{T}X)^{-1}X^{T}X(X^{T}X)^{-1}X^{T} \\
	&= X(X^{T}X)^{-1}X^{T} \\
	&= H
\end{align}
$$
```

$H$ is an orthogonal projection matrix onto the **column space** of $X$. Geometrically, it projects the responses $y$ onto the column space of $X$, returning  
$$\hat{y}=Hy=X\hat{\beta}$$ 
Here, the column space of $X$ is the set of vectors a which may be expressed as $a = Xb$ for some $b  \in \mathbb{R}^{p+1}$.

The projection $\hat{y}=Hy=X\hat{\beta}$  is the vector that is **CLOSEST** (in terms of Euclidean distance) to $y$, subject to **residing in the column space** of $X$.

![[Pasted image 20240904132200.png|500]]

As described here ![[2 Multiple Regression#^9294d3]]
The OLS method aims to **minimize** the Euclidean distance between $y$ and the linear space spanned by the columns of $X$.

```ad-important
**Definition 3.3**: Orthogonality of Residuals and Predictors

Intuitively and as shown in the diagram above, for any vector $a \in \mathbb{R}^{n}$ in the column space of $X$ (i.e. $a=Xb$ for some vector $b \in \mathbb{R}^{p+1}$)m we have
$$a^{T}e=0$$

In other words, the residual is orthogonal to all vectors in the column space of $X$.
```

The intuition here is that residuals are what’s *left over* after running our regression, that which **cannot be predicted or explained** by a linear function of our predictor variables ($X$).

---
### Properties of $\hat{\beta}$
With some algebraic properties of the ordinary least squares solution established, we’ll next investigate statistical properties of $\hat{\beta}$ when viewed as an estimator of $\beta$.