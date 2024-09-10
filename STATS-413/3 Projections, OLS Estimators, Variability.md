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

Again, the variability in responses $y$ we observed in the model stems from the different vector $\epsilon$ - this in turn generates different slope and intercept coefficients when running regression.

Let's first go over a few definitions and properties. Let $Y, Z \in \mathbb{R}^{m}$ be a **random** vector.

```ad-important
Definition 3.4: Properties of Expectations

For $A \in \mathbb{R}^{n\times m}, b \in \mathbb{R}^{n}$, $A, b$ constant,
$$\mathbb{E}(AY+b)=A\mathbb{E}(y)+b$$
$$\mathbb{E}(Y+Z)=\mathbb{E}(Y)+\mathbb{E}(Z)$$
```

```ad-important
**Definition 3.5**: Variance, Covariance

For some $Z \in \mathbb{R}^{m}$,
$$\begin{align}\text{Var}(Z)&=\mathbb{E}[(Z-\mathbb{E}(Z))(Z-\mathbb{E}(Z))^{T}]
\\&= \mathbb{E}(ZZ^{T})-\mathbb{E}(Z)\mathbb{E}(Z)^{T}
\\&= \begin{bmatrix} \text{Var}(Z_{1}) & \dots & \dots & \text{Cov}(Z_{1},Z_{m}) \\ \text{Cov}(Z_{2},Z_{1}) & \text{Var}{Z_{2}} &\dots & \text{Cov}(Z_{2},Z_{m})  \\ \vdots & \ddots & \ddots &\vdots \\ \text{Cov}(Z_{m},Z_{1}) &\dots &\dots & \text{Var}(Z_{m}) \end{bmatrix}
\end{align}$$

If ${Z_{j}}$ are independent, $\text{Var}(Z) = \text{Diag}(\text{Var}(Z_{j}))$.
```

```ad-important
**Definition 3.6**: Properties of Variance

For $A \in \mathbb{R}^{n \times m}, b \in \mathbb{R}^{n}$, $A, b$ constant,
$$\text{Var}(AZ+b)=A\text{Var}(Z) A^{T}$$
```

Using the theorems developed above, we may derive the expectation of $\hat{\beta}$: $$\begin{align}\mathbb{E}(\hat{\beta})&=\mathbb{E}((X^{T}X)^{-1}X^{T}y)\\&=(X^{T}X)^{-1}X^{T}\mathbb{E}(y) \\ &= (X^{T}X)^{-1}(X^{T} X)\beta \\&= \beta \end{align}$$
This demonstrates the **unbiasedness** of $\hat{\beta}$. ^a4deed
- Here, we rely on the fact $\mathbb{E}(\epsilon)=0$ since it implies $\mathbb{E}(y)=X\beta$

Similarly, we may define the variance of $\hat{\beta}$: $$\begin{align} \text{Var}(\hat{\beta})&= \text{Var}((X^{T}X)^{-1}X^{T}y) \\ &= (X^{T}X)^{-1}X^{T} \text{Var}(y) ((X^{T}X)^{-1}X^{T})^{T} \\&=\sigma_{\epsilon}^{2}(X^{T}X)^{-1} \end{align}$$
Here, we rely on the fact $\text{Var}(\epsilon)=\sigma_{\epsilon}^{2}I$. ^89d509

Note that $\text{Var}(\hat{\beta})$ is a $(p+1) \times (p+1)$ matrix, with the variances of each individual slope coefficient on the diagonal - the $j$ th slope coefficient, look at the $(j+1)$ st diagonal element.

---
### Variability in Residuals
We’ll now introduce a few summary measures related to variability in the residuals in order to:
1. Characterize **how much variability in our outcomes** cannot be accounted for by our predictor variables.
2. Assess **quality of model fit**.
3. Come up with an estimator for $\sigma_{\epsilon}^{2}$, a necessary step when (a) performing inference; and (b) constructing prediction intervals for future observations.

```ad-important
**Definition 3.7**: Residual Sum of Squares (RSS)
$$RSS=\sum\limits_{i=1}^{n} e_{i}^{2}=\sum\limits_{i=1}^{n} (y-\hat{y})^{2}=(y-X\hat{\beta})(y-X\hat{\beta})^{T}=e^Te$$

Note that this is also known as the *sum of squared errors*, introduced [[1 Simple Regression#^302e2f|here]]. This is also how we derived the OLS estimator $\hat{\beta}$.
```

```ad-important
**Definition 3.8**: Total Sum of Squares (TSS)

The total sum of squares represents the total **variability** in $y$ **itself**, **WITHOUT** trying to predict using our explanatory variables.
$$TSS = \sum\limits_{i=1}^{n} (y_{i}-\bar{y})^{2}=(n-1)\text{Var}(y)$$

where $\text{Var}(y)$ is the sample variance of $y$.
```

Suppose the case where we don't have any predictors, $\hat{y}_{i}=\overline{y}$ for all individuals, implying that the residual would be $y_{i}-\overline{y}$. Hence, TSS demonstrates the total variability of our data, in absence of any predictors (worst regression model). 

In other words, the TSS is the sum of squared residuals in a regression that **DOES NOT include any of the predictor variables**, and instead only uses an intercept; while RSS is the sum of squared residuals in a regression that **DOES** **use the predictor variables**.

This leads us to $R^{2}$ - the coefficient of determination.

---
### $R^{2}$

```ad-important
**Definition 3.9**: $R^{2}$

$$R^{2}=1- \frac{RSS}{TSS}$$

The $R^{2}$ value is the fraction of variation in the outcome variable $y$ that **can be accounted** for by our predictor variables $X$ through our linear model.

It quantifies how much “better” we are doing in prediction by using our linear model than we would have been stuck with in the absence of any predictor variables.

Additionally,
$$R^{2}= \text{cor}(\hat{y},y)$$

In the case of simple regression ($p=1$), $R^{2}=\text{cor}(x,y)$.
```

It's easy to note that $0 \le R^{2} \le 1$ since $RSS \le TSS$. 

$R^{2}$ **NEVER decreases** as you add more predictor variables in to your regression (even if the predictors are irrelevant!).

![[Pasted image 20240905120329.png|400]]

It only measures the **strength of the LINEAR relationship** between responses and predictors. Not an appropriate summary measure under model misspecification.

### Further Readings
- [Expectation and Variance Derivation in Matrix Form](https://www.sfu.ca/~lockhart/richard/350/08_2/lectures/GeneralTheory/web.pdf)