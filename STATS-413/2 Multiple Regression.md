[[2024-08-28]] #Regression 

In general, our responses $y_i$ might be thought of as functions of $p > 1$ explanatory variables

We’d like to fit a function of the form:
$$\hat{y}_{i}=\hat{\beta}_{0}+\hat{\beta}_{1}x_{i1}+\hat{\beta}_{2}x_{i2}+\cdots+\hat{\beta}_{p}x_{ip}$$
Similar to how we solve the optimization problem with one predictor variable: ![[1 Simple Regression#^302e2f|1 Intro]]
We may extend this to multiple dimensions, and hence we would come up with a similar optimization problem: 

The **least squares coefficients** $(\tilde{\beta_0},\tilde{\beta_1}, \cdots, \tilde{\beta_p})$ to minimize the sum of the squared errors (SSE):
$$(\hat{\beta_{0}},\hat{\beta}_{1}, \cdots, \hat{\beta}_{p})=\text{arg min}_{\tilde{\beta_0}, \tilde{\beta_1}, \cdots, \tilde{\beta_p}} \sum\limits_{i}^{n}(y_{i}-(\tilde{\beta_0}+\tilde{\beta_1}x_{i1}+\cdots + \tilde{\beta_p}x_{ip}))^{2}$$
The **fitted model**, for forming predictions at other values of x beyond those in our data, is
$$\hat{f}(x^\star)=\hat{\beta_0}+\hat{\beta_1}x_{1}^{\star}+\cdots+\hat{\beta_p}x_{p}^{\star}$$
And residuals (observation minus the prediction) are
$$e_{i}=y_{i}-\hat{y}_i$$
For $i=1,...n$.

### General Objective in Regression 
We begin by postulating a **generative model**, which describes how our observed data $(x_{1}, y_{1}),\cdots, (x_{n}, y_{n})$ came to be. We started with values for our **predictor variables**: $x_{1}, ... x_{n}$ and observe different values for $y_{1}, ... y_{n}$ because of randomness $\epsilon_{1}, ..., \epsilon_{n}$.
- Relate the **observed data to parameters of our population of interest**
- Through using the data, we try to **infer the values of the parameters** for the population of interest 

```ad-important
**Definition 2.1**: Regression, General Form

General form for this generative model states that, for $i = 1, \cdots, n$,
$$y_{i}=f(x_{i})+\epsilon_{i}$$
for $i=1,\cdots, n$, where $f(x_{i})=\mathbb{E}(y_{i}|x_{i})$ which is the signal relating expected response to predictors, and $\epsilon_i$ are **random noise terms**, or **errors**, with $\mathbb{E} (\epsilon_{i}|x_{i})=0$.

Our objectives are:
- Estimate $f(\cdot)$ using observed data, resulting in estimated function $\hat{f(\cdot)}$
- Estimate the conditional distribution $Y_{i}|x_{i}$
```

We generally impose some restrictions/structure on $f (·)$.
- For **linear models**, we assume $$f(x_{i})=\mathbb{E}(y_{i}|x_{i})=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}$$ where $\beta_{j}$ 's are **unknown parameters** and $\beta_{0}$ is the intercept
	- The *true value* that we aim to estimate
- Hence, estimation of $f(\cdot)$ is reduced to the **estimation** of $\beta_j$ 's

Linear models can also be made flexible by **transformation** of the response and the predictors.
- For example, if we want to use linear regression to fit $\hat{f}(x)=\hat{\beta_0}+\hat{\beta_1}x+\hat{\beta_{2}}x^2$, we can simply let $x_{2}=x_{1}^{2}$ then plug in to the same linear model

#### Weaker Linear Regression
In addition to the model demonstrated above, we have the following assumptions:
1. **Linearity**: the true relationship between the conditional expectation of $y$ and the **predictors** is **linear**
	- $f(x_{i})=\mathbb{E}(y_{i}|x_{i})=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}$ 
2. **Homoskedasticity**: the error terms have the same variance
	- $\text{Var}(\epsilon_{i} | x_{i}) = \sigma_{\epsilon}^2$ for all individuals
	- If the variability of the noise instead changes as a function of the covariates, we call the noise **heteroskedastic**
3. **Uncorrelated noise**: $\text{Cov}(\epsilon_{i},\epsilon_{j}|x_{i},x_{j})=0$ for $i \ne j$

For now, we don’t assume that $\epsilon_{i}$ follow a **particular distribution** (for instance, normal). The model will be strengthened later when discussing statistical inference.

Under this regression model, we view the values of $(x_1,..., x_n)$ as **fixed quantities**. In other words, we proceed by **conditioning upon** the observed values of $(x_1,..., x_n)$ in the sample at hand. Though these are *fixed values*, the outcome is still random due to their dependence on $\epsilon_{i}$
- The regression models the **conditional distribution** of $Y$ given $X=x$

```ad-note
Throughout the semester, we’ll be **conditioning on the covariates** used in the regression model when making probabilistic statements; e.g. by default $\mathbb{E}(\epsilon_{i})$ is equivalent to $\mathbb{E}(\epsilon_{i}|x_{i})$.
```

---
### Estimation for Linear Models

```ad-important
**Definition 2.2**: Multiple Regression Model

The model is $$y_{i}=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}+\epsilon_{i}$$
for $i=1,...,n$.

Our goal is given $(x_{i},y_{i})$, $i=1,...,n$, estimate $\beta_{0}, ...,\beta_{p}$. We adopt the same least squares criterion based on minimizing $$\sum\limits_{i}^{n}(y_{i}-(\tilde{\beta_0}+\tilde{\beta_1}x_{i1}+\cdots + \tilde{\beta_p}x_{ip}))^{2}$$

![[Pasted image 20240901215717.png|400]]
```

Moving forwards, we adopt matrix notation for the preceding linear regression model. Let
$$y=\begin{bmatrix}y_{1} \\ \vdots \\ y_{n} \end{bmatrix}, X=\begin{bmatrix} 1 & x_{11} & \dots & x_{1p} \\ \vdots & \vdots & x_{ij} & \vdots \\ 1 & x_{n1} & \dots & x_{np} \end{bmatrix}$$
The matrix $X$ is of size $(n \times p+1)$, the extra column is to account for the **intercepts**. $X$ is called the **design matrix**, or **model matrix**. 

In addition, let $$\beta=\begin{bmatrix} \beta_{0} \\ \vdots \\ \beta_{p} \end{bmatrix},\epsilon=\begin{bmatrix} \epsilon_{0} \\ \vdots \\ \epsilon_{n} \end{bmatrix} $$
Such that we have 
$$Y=X\beta+\epsilon$$
as our linear model.

Note that $\mathbb{E}(Y)=X\beta \Longleftrightarrow \mathbb{E}(\epsilon)=0$ since with a fixed $X$, the error term $\epsilon$ is the only factor that contributes to the randomness of our model.

```ad-note
**Expectation and Variances of Column Vectors**
$$\mathbb{E}(y)= \mathbb{E}\left(\begin{bmatrix} y_{1} \\ \vdots \\ y_{n} \end{bmatrix}\right)=\begin{bmatrix} \mathbb{E}(y_{1}) \\ \vdots \\ \mathbb{E}(y_{n}) \end{bmatrix}$$

$$\text{Var}(y)=\begin{bmatrix} \text{Var}(y_{1}) & \dots & \text{Cov}(y_{1},y_{n}) \\ \vdots & \text{Cov}(y_{i},y_{j}) & \vdots \\ \text{Cov}(y_{n}, y_{1}) & \dots & \text{Var}(y_{n})\end{bmatrix}$$
```

Note that $\text{Var}(y)=\text{Var}(\epsilon)=\sigma_{\epsilon}^{2} I_{n \times n}$.

We can also write the **least squares criterion** in matrix form:
$$\begin{align}\min_{\tilde{\beta}}{\sum\limits_{i=1}^{n}e_{i}^{2}}&=\min_{\tilde{\beta}}e^{T}e \\ &= \min_{\tilde{\beta}} (y-X\tilde{\beta})^T(y-X\tilde{\beta})
\end{align}$$
Then, we differentiate the criterion with respect to $\tilde{\beta}$ - attaining
$$\frac{\partial}{\partial \tilde{\beta}} (y-X\tilde{\beta})^T(y-X\tilde{\beta})=-2X^{T}y+2(X^{T}X)\tilde{\beta}$$
Setting this to zero allows us to find the minimizer (our objective function is convex).

```ad-important
**Definition 2.3**: Normal Equation

The linear regression estimator $\hat{\beta}$ must satisfy that
$$X^{T}X\hat{\beta}=X^{T}y$$

As long as $X^{T}X$ is **invertible**, i.e. $\text{rank}(X)=p+1$, we will have a unique solution.
- This constraint may be failed under two conditions
	1. $p+1 > n$ (more predictor variables than we have observations)
	2. Collinearity (a column of $X$ can be expressed as a linear combination of the others)
```

^81c207

```ad-important
**Definition 2.4**: OLS Estimators

If $X^{T}X$ is invertible, the unique solution of [[#^81c207|Definition 2,3]] is
$$\hat{\beta}_{(p+1) \times 1}=(X^{T}X)^{-1}X^{T}y$$
```

Some additional quantities that may be represented in now the vector/matrix form:
- Fitted values
	- $\hat{y}_{i}=x_{i}^{T}\hat{\beta}$
	- $\hat{y}=X\hat{\beta}$
- Fitted models
	- $\hat{f}(x^\star)=\hat{\beta_0}+\hat{\beta_1}x_{1}^{\star}+\cdots+\hat{\beta_p}x_{p}^{\star}=(x^{\star})^{T}\hat{\beta}$
- Residuals
	- $e_{i}=y_{i}-\hat{y}_{i}=y_{i}-x_{i}^{T}\hat{\beta}$
	- $e=y-\hat{y}=y-X\hat{\beta}$

Note that the residual $e_{i}$ are different from the error terms $\epsilon_{i}$.

---
### Interpretation of Intercepts
$\beta_{0}$, by extrapolation, is the *true value* for $\mathbb{E}(y_{i}|x_{i})$ when $x_{i}=\mathbf{0}$ ; $\hat{\beta_{0}}$ is its estimate. The intercept provides the proper vertical placement for the mean function $\mathbb{E}(y_{i}|x_{i})$.

Slope coefficients $(\beta_{1},..., \beta_{p})$ provide **comparisons** between the **expectations** at different points $x$. **Holding all other variables equal (ceteris paribus)**, two individuals who differ in variable $x_{j}$ by $1$ unit are expected to differ in $Y$ by $\beta_j$ units. $\hat{\beta}_{j}$ is our estimate of this difference in expectations.

Slope is **NOT changes** as it ascribes a causal interpretation to the slope and overlooks the effect of omitted variables.

---
### Covered R Functions

```r
# Generate Scatter Plot Matrix
pairs(cbind(sales, income, competitors))

# Generate Correlation Matrix
cor(cbind(sales, income, competitors))

# Multiple regression syntax
lm(y~x1+x2+...+xp)

# Summary of Regression 
summary(lm)
```