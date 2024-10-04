[[2024-10-01]] #Regression 

### Multicollinearity
Multicollinearity/Collinearity refers to a situation where two or more predictor variables are strongly linearly related with one another. Let $$X=\begin{bmatrix} | & | & \cdots & | \\ 1 & x_{1} & \cdots & x_p \\ | & | & \cdots &| \end{bmatrix}$$
**Perfect multicollinearity** is rare: requires that one predictor variable $x_j$ can be expressed as a linear combination of **the other** predictors: $$x_{j}=\alpha_{0}+\alpha_{1}x_{1}+\cdots+\alpha_{p}x_p$$
Avoiding perfect multicollinearity came up when [[6 Categorical Variables#^2b4af9|encoding categorical predictors]] as $G-1$ dummy variables.

More frequently, our concern is with **strong**, not perfect, collinearity. $$x_{j}\approx \alpha_{0}+\alpha_{1}x_{1}+\cdots+\alpha_{p}x_p$$
```ad-note
**Multicollinearity vs Leverage**

Both multicollinearity and leverage have to do with the distribution of the **predictors**, not the responses.

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
We’ll now illustrate a different approach. To do this, let $X_{R}$ be a $n \times p$ matrix containing all of the predictor variables **except** the $j$ th predictor (the “rest” of the predictor variables besides $j$). $$X_{R}=\begin{bmatrix}| & | & \cdots & | & | & \cdots & |  \\ 1 & x_{1} & \cdots & x_{j-1} & x_{j+1} & \cdots & x_{p} \\ | & | & \cdots & | & | & \cdots & |\end{bmatrix}$$ Let $H_{R}$ be the hat matrix corresponding to $X_{R}$: $$H_{R}=X_{R}(X_{R}^T X_{R})^{-1}X_{R}^T$$
Consider running the following two regressions:
1. $y$ on $X_{R}$
2. $x_{j}$ on $X_{R}$

Define the residuals from these regression as $$y_{.R}=(I-H_{R})y$$ and $$x_{j.R}=(I-H_{R})x_{j}$$
Now, consider running a simple regression of $y_{.R}$ on $x_{j.R}$.
- $y_{.R}$ is the component of y that **CANNOT be explained** as a linear function of the covariates $X_{R}$; it is $y$ adjusted for $X_{R}$
- $x_{j.R}$ is the component of $x_{j}$ that **CANNOT be explained** as a linear function of the covariates $X_{R}$; it is $x_{j}$ adjusted for $X_{R}$

```ad-note
Note that $\bar{y}_{.R}=\bar{x}_{j.R}=0$ because they are **averages of residuals** from regressions including an intercept term.
```

Hence, we have that $$\hat{\beta}_{0.R}=0$$ and $$\hat{\beta}_{j.R}=\frac{\sum_{i=1}^{n}x_{ij.R}y_{i.R}}{\sum_{i=1}^{n}x_{ij.R}^2}=\frac{x_{j}^{T}(I-H_{R})y}{x_{j}^{T}(I-H_{R})x_{j}}$$ 
```ad-important
**Definition 11.1**: Multiple Regression Slopes Through Partial Regression

Let $\hat{\beta}_{j}$ be the $j+1$th coordinate of $\hat{\beta}$. $$\hat{\beta}_{j.R}=\hat{\beta}_{j}$$
```

```ad-note
**Proof of Definition 11.1**

$$\begin{align}
y &= X\hat{\beta} + e\\
&= X\hat{\beta}+(I-H)y\\
\to (I-H_{R})y&=(I-H_{R})X\hat{\beta}+(I-H_{R})(I-H)y & {\text{ Multiply by }} (I-H_{R})
\end{align}$$
Observe that $$\begin{align}
(I-H_R)X_{R} &= 0\\
(I-H_{R})(I-H) &= (I-H)
\end{align}$$ since $X_{R}$ reside in the columnspace of $X$, so that $X_{R}H=X_R$ and $H_{R}H=H_{R}$.
``
Hence the above becomes $$y_{.R}=x_{j.R}\hat{\beta}_{j}+(I-H)y$$ Subtract $x_{j.R}\hat{\beta}_{j}$ from both sides and multiply by $x_{j}^{T}(I-H_{R})$ yields $$x_{j.R}^{T}(y_{.R}-x_{j.R}\hat{\beta}_{j})=x_{j}^{T}(I-H_{R})(I-H)y$$ The RHS equals **zero** since $x_{j}^T (I-H)y=0$ as $x_{j}$ is in the columnspace of $X$.
```

The above implies that $\hat{\beta}_{j}$ **satisfies the normal equations** for a regression of $y_{.R}$ on $x_{j.R}$.

```ad-summary
In a simple regression of $y_{.R}$ on $x_{j.R}$.
- Intercept is **zero**
- Slope is $\hat{\beta}_{j}$
- Residuals are **identical** to the residuals in a regression of $y$ on $X$ (shown by this: $y_{.R}=x_{j.R}\hat{\beta}_{j}+(I-H)y$)
```

A scatterplot of $y_{.R}$ against $x_{j.R}$ is called a **partial regression plot**. It can be used as an additional diagnostic tool to detect
- Nonlinearity
- Heteroskedasticity
- **Influential points** for determining the $j$ th slope coefficient

Because $\hat{\beta}_{j.R}=\hat{\beta}_{j}$, it must be that $$\text{Var}(\hat{\beta}_{j})=\text{Var}(\hat{\beta}_{j.R})$$ Thus, we can rewrite $\text{Var}(\hat{\beta}_{j})=\sigma_{\epsilon}^{2}(X^TX)^{-1}_{(j+1),(j+1)}$  as $$\begin{align}
\text{Var}(\hat{\beta}_{j})&=\frac{\sigma_{\epsilon}^{2}}{x_{j}^{T}(I-H_{R})x_{j}} \\\\
&= \frac{\sigma_{\epsilon}^{2}}{\sum_{i=1}^{n}x_{ij.R}^{2}}
\end{align}$$
This result provides another (equivalent) way to compute **standard errors**: $$
\text{se}(\hat{\beta}_{j})=\frac{\hat{\sigma}_{\epsilon}}{\sqrt{x_{j}^{T}(I-H_{R})x_{j}}}$$ That said, the main reason we’ve gone through partial regression is that it inspires a useful measure of multicollinearity.
- Measure how “unstable” a slope coefficient is because of multicollinearity
- Instability = High variance
- *How much has the variability in the slope coefficient increased due to multicollinearity?*

We think of $x_{j}^{T}(I-H_{R})x_{j}$ as a **type of RSS** from our partial regression framework ($x_{j}$ on $X_{R}$): $$ RSS_{j}=x_{j}^{T}(I-H_{R}) x_{j}=\sum\limits_{i=1}^{n} (x_{ij}-\hat{x}_{ij})^{2}$$ where $\hat{x}_{ij}$ are my predicted values from a regression of $x_{ij}$ on $X_{R}$.

The corresponding **TSS** for this partial regression is $$TSS_{j}=\sum\limits_{i=1}^{n}(x_{ij}-\bar{x}_{j})^{2}=(n-1)\text{var}(x_{j})$$ where $\text{var}(x_{j})$ is the sample variance.

```ad-note
Generally, we prefer RSS to be small relative to TSS.
- In this context however, $RSS_j$ small relative to $TSS_j$ indicates **strong multicollinearity**!
- It means we can predict $x_j$ very well as a linear function of the other predictor variables

Instead, we want RSS to be as close to TSS as possible.
```

There would be **NO multicollinearity** issues with respect to $x_{j}$ and $\hat{\beta}_{j}$ if $$RSS_{j}=TSS_{j}$$
This would occur if $$\frac{1}{n}\sum\limits_{i=1}^{n} (x_{ij}-\bar{x}_{j})(x_{ik}-\bar{x}_{k})=0$$ for $k=1,\dots, p$; that is, if $x_{j}$ minus its **sample mean** is **orthogonal** to the other $p-1$ predictor variables minus their sample means.

If $x_{j}-\bar{x}_{j}$ is orthogonal to $x_{k}-\bar{x}_{k}$ for $k=1,\dots, p$, $k\ne j$: $$x_{ij.R}=x_{ij}-\bar{x}_{j}$$
```ad-note
The above effectively says that the residual of $x_{j}$ when regressed on $X_{R}$ is equal to the predictor $x_{j}$ subtracted by the sample mean of $x_{j}$.
- Our prediction for all $n$ observations of $x_{j}$ is just its sample mean, hence $RSS_{j}=TSS_{j}$
- This implies that we are **NOT** better off at all regressing $x_{j}$ on $X_{R}$ and hence we predict everything based on its sample mean, meaning that $X_{R}$ gives us **NO information** - $x_{j}$ and all other predictors are not related
```

In this case, the following two slopes are **identical**:
1. The estimated slope coefficient on $x_{j}$ in a simple regression of $y$ on $x_j$
2. The estimated slope coefficient on $x_{j}$ in a multiple regression of $y$ on $x_{1}, x_{2},\dots, x_{p}$
