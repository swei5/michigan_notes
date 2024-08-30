[[2024-08-28]] #Regression 

In general, our responses $y_i$ might be thought of as functions of $p > 1$ explanatory variables

We’d like to fit a function of the form:
$$\hat{y}_{i}=\hat{\beta}_{0}+\hat{\beta}_{1}x_{i1}+\hat{\beta}_{2}x_{i2}+\cdots+\hat{\beta}_{p}x_{ip}$$
Similar to how we solve the optimization problem with one predictor variable: ![[1 Simple Regression#^302e2f|1 Intro]]
We may extend this to multiple dimensions, and hence we would come up with a similar optimization problem: 

The **least squares coefficients** $(\beta_{0},\beta_{1}, \cdots, \beta_{p})$ to minimize the sum of the squared errors (SSE):
$$(\hat{\beta_{0}},\hat{\beta}_{1}, \cdots, \hat{\beta}_{p})=\text{arg min}_{\beta_{0}, \beta_{1}, \cdots, \beta_{p}} \sum\limits_{i}^{n}(y_{i}-(\beta_{0}+\beta_{1}x_{i1}+\cdots + \beta_{p}x_{ip}))^{2}$$

### General Objective in Regression 
We begin by postulating a **generative model**, which describes how our observed data $(x_{1}, y_{1}),\cdots, (x_{n}, y_{n})$ came to be.
- Relate the **observed data to parameters of our population of interest**
- Through using the data, we try to **infer the values of the parameters** for the population of interest 

```ad-important
**Definition 2.1**: Regression, General Form

General form for this generative model states that, for $i = 1, \cdots, n$,
$$y_{i}=f(x_{i})+\epsilon_{i}$$
for $i=1,\cdots, n$, where $f(x_{i})=\mathbb{E}(y_{i}|x_{i})$ which is the signal relating expected response to predictors, and $\epsilon_i$ are random noise terms, with $\mathbb{E} (\epsilon_{i}|x_{i})=0$.

Our objectives are:
- Estimate $f(\cdot)$ using observed data, resulting in estimated function $\hat{f(.)}$
- Estimate the conditional distribution $Y_{i}|x_{i}$
```

We generally impose some restrictions/structure on $f (·)$.
- For linear models, we assume $$f(x_{i})=\mathbb{E}(y_{i}|x_{i})=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}$$ where $\beta_{j}$ 's are **unknown parameters** and $\beta_{0}$ is the intercept.
- Hence, estimation of $f(.)$ is reduced to the **estimation** of $\beta_j$ 's

