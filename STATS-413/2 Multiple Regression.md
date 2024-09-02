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
We begin by postulating a **generative model**, which describes how our observed data $(x_{1}, y_{1}),\cdots, (x_{n}, y_{n})$ came to be.
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