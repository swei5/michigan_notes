[[2024-10-01]] #Regression 

### Multicollinearity
Multicollinearity/Collinearity refers to a situation where two or more predictor variables are strongly linearly related with one another. Let $$X=\begin{bmatrix} | & | & \cdots & | \\ 1 & x_{1} & \cdots & x_p \\ | & | & \cdots &| \end{bmatrix}$$
**Perfect multicollinearity** is rare: requires that one predictor variable $x_j$ can be expressed as a linear combination of **the other** predictors: $$x_{j}=\alpha_{0}+\alpha_{1}x_{1}+\cdots+\alpha_{p}x_p$$
Avoiding perfect multicollinearity came up when [[6 Categorical Variables#^2b4af9|encoding categorical predictors]] as $G-1$ dummy variables.

More frequently, our concern is with **strong**, not perfect, collinearity. $$x_{j}\approx \alpha_{0}+\alpha_{1}x_{1}+\cdots+\alpha_{p}x_p$$
```ad-note
**Multicollinearity vs Leverage**

Both multicollinearty and leverage have to do with the distribution of the **predictors**, not the responses.

They describe very different attributes of predictor space, with different statistical ramifications.
- Leverage: certain observations are outliers with respect to the distribution of $X$; have potential to exert large influence over the OLS solution
- Multicollinearity: predictor space is nearly degenerate; any predictor $x_i$ “nearly” resides in a **lower-dimensional subspace** of $\mathbb{R}^{p}$
```

#### Consequences of Collinearity
Mathematically, if $X^{T}X$ is **close to singular**, $(X^{T}X)^{-1}$ is **UNSTABLE**.
- Small changes in $X$ can have a large impact on $(X^{T}X)^{-1}$

Recall that $$\hat{\beta}=(X^{T}X)^{-1}X^{T}y$$ Because $\hat{\beta}$ depends on $(X^{T}X)^{-1}$, collinearity has many statistical ramifications.
- Imprecise / unstable estimates of $\beta$
- Strong sensitivity of $\hat{\beta}$ to rounding of predictor variables, or slight measurement errors
- $t$ -tests can fail to reveal significant predictors
- Coefficients can have weird signs and values relative to intuition
- Estimated predictions $\tilde{x}^{T}\hat{\beta}$, highly unstable

Pairwise **correlations and scatterplots** help us assess if any pairs of predictor variables are **strongly related**. 

![[Pasted image 20241003175625.png|400]]
![[Pasted image 20241003175651.png|400]]

However, pairwise scatterplots and correlations won’t reveal if one predictor is (nearly) a linear combination of **multiple predictor variables**. To devise a metric for assessing multicollinearity more generally, we’ll now dive into the role that multicollinearity plays in determining $\hat{\beta}_{k}$ and its standard error.

---
### Partial Regression
To calculate the $j$ th estimated slope coefficient $\hat{\beta}$, we simply use the $j+1$ th coordinate of $$\hat{\beta}=(X^{T}X)^{-1}X^{T}y$$
We’ll now illustrate a different approach. To do this, let $X_{R}$ be a $n \times p$ matrix containing all of the predictor variables **except** the $j$ th predictor (the “rest” of the predictor variables besides $j$). $$X_{R}=\begin{bmatrix}| & | & \cdots & | & | & \cdots & |  \\ 1 & x_{1} & \cdots & x_{j-1} & x_{j+1} & \cdots & x_{p} \\ | & | & \cdots & | & | & \cdots & |\end{bmatrix}$$
