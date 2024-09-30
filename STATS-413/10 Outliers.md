[[2024-09-26]]  #Regression

### Recap: Importance of LR Assumptions
Consider our test statistic for testing the null $H_{0}:\beta_{j}=\gamma_{0}$ with a two-sided alternative: ![[4 Hypothesis Tests#^cecd4f]]
Here **linearity** plays a role in asserting the **unbiasedness** of $\hat{\beta}$ and thus the center of the distribution. **Homoskedasticity** plays a role in establishing constant $\hat{\sigma}_{\epsilon}^{2}$ and therefore the standard error. **Normality** plays a role in forming the $T$ distribution.

In addition to our suite of regression diagnostics assessing linearity, homoskedasticity, and normality, we’ll introduce additional diagnostics to help us **flag potentially unusual / important observations** in our data set.
- **Outliers** - observations do not behave like the bulk of our data points
- **Influential points** - observations which unduly/substantially affect the prediction equation

A data point could be none, one, or both of these.

### Outliers
Informally, an outlier is an observation that differs substantially from the other points in the data set.
- Mistakes
- A rare, but possible, outcome in our population

Including an **additional predictor variable** can also reveal outliers that weren’t apparent before.

Naively, our first thought may be to look for observations with extremely large or small residuals, $e_{i}=y_{i}-x_{i}^{T}\hat{\beta}$
- Hope: individuals who deviate substantially from the majority of the data should have residuals that *stand out*

However, this might not always be working out...

![[Pasted image 20240930110929.png|400]]
In the first example, since the data point has much **influence** over the regression and hence reducing its residuals to be similar to the others.

---
### Leverage
Individual observations vary in the influence they have over the fitted values, and over the estimated regression line.
- Previously, we saw that whether a not a data point’s predictor value is extreme plays a key role here

```ad-important
**Definition 10.1**: Leverage

We define the leverage, or self-influence, of the $i$ th observation (with covariate $x_i$ ) as $$\text{Leverage}_{i}=h_{ii}$$ where $h_{ii}$ is the $i$th diagonal entry of the hat matrix, $H=X(X^{T}X)^{-1}X^{T}$. Thus, we have $$h_{ii}=x_{i}^{T}(X^{T}X)^{-1}x_{i}$$
```

Leverage thus depends not only on $x_{i}$ , but on the predictor variables for the other $n-1$ individuals in the dataset.

Here, $h_{ii}$ serves as a measure of how much the $i$ th individual’s observed value, $y_{i}$, influences the $i$ th observation's fitted value $\hat{y}_{i}$. $$\begin{align}\hat{y}&=Hy \\ \hat{y_{i}} &=\sum\limits_{i=1}^{n}h_{ij} y_{j} \\&=h_{ii}y_{i}+\sum\limits_{j\ne i} h_{ij}y_{j} \end{align}$$
Recall that previously we've assumed the following properties when conducting a **regression with an intercept** (note that we haven't really proven this yet)
1.  $\frac{1}{n}\le h_{ii}\le1$
2.  $\sum_{i=1}^{n}h_{ii}=p+1$
3.  $h_{ii}(1-h_{ii})=\sum_{i\ne j} h_{ij}^{2}$ 

Here $h_{ii}$ can be thought of as the **weight** placed on $y_{i}$ when forming $\hat{y}_{i}$, and $\sum_{j\ne i} h_{ij}y_{j}$ is the contribution of other $n-1$ observations.

Let's consider the extreme case where $h_{ii}=1$. Then,
- $\hat{y}_{i}=y_{i}$ since each $h_{ij}=0$ from (3)
- $e_{i}=y_{i}-\hat{y}_{i}=0$
- $\text{Var}(\hat{y}_{i} | x_{i})=\sigma_{\epsilon}^{2}h_{ii}=\sigma_{\epsilon}^{2}$
- $\text{Var}(e_{i}|x_{i})=\sigma_{\epsilon}^{2}(1-h_{ii})=0$

```ad-summary
**Leverage and Linear Model Assumptions**

Under the weaker linear model we have that $$\text{Var}(e_{i}|x_{i})=\sigma_{\epsilon}^{2}(1-h_{ii})$$ $$\text{Var}(\hat{y}_{i} | x_{i})=\sigma_{\epsilon}^{2}h_{ii}$$ $$\text{Corr}(\hat{y}_{i},y_{i}|x_{i})=\sqrt{h_{ii}}$$

The larger $h_{ii}$, the smaller the variability in the $i$th residual, $e_{i}$ and the closer $y_{i}$ tends to be to $\hat{y}_{i}$, or the more $\hat{y}_{i}$ behaves like $y_{i}$
```

Through a bit of linear algebra (no need to derive), we can derive an alternative form for hii that helps understand what values of $x_{i}$ have high leverage. $$h_{ii}=\frac{1}{n}+(x_{i\backslash0}-\bar{x}_{i \backslash 0})^{T}(X_{\backslash 0}^T (1-H_{0})X_{\backslash 0})^{-1}(x_{i\backslash0}-\bar{x}_{i \backslash 0})$$ where $X_{\backslash 0}$ is the design matrix **WITHOUT** the **intercept column** ($p \times n$ matrix), $x_{i \backslash 0}$ are the predictor variables for the $i$ th individual with the intercept entry excluded, and $\bar{x}_{\backslash 0}=[\bar{x}_{1},\dots, \bar{x}_{p}]^{T}$ are the means of the predictor variables.

Note that the second term is always **non-negative**. If $x_{i}=\bar{x}$, then the second term is cancel out and $h_{ii}=\frac{1}{n}$.

```ad-summary
**Observations**

We see that $x_{i}=\bar{x}$ results in the **smallest possible self-influence**, $\frac{1}{n}$. In addition, as $x_{i}$ moves farther away from $\bar{x}$, the leverage **increases**.

In sum, points with high leverage are outliers **with respect to the distribution of the predictor variables**.
```

```ad-example
**Example**: Leverages and Outliers

![[Pasted image 20240930135518.png|400]]
```

In summary, leverages $h_{ii}$ provide a scalar/numeric representation of the “self-influence” of a point.
- How much does the observation $y_{i}$ determine the fitted value $\hat{y}_{i}$?

```ad-note
Leverages essentially help us answer this question: *At what points $x_{i}$ would an outlier in my responses $y_{i}$ have the **potential to dramatically alter** my prediction equation?*
```

We see it is a function of the **predictor variables ALONE**, as it is based on the hat matrix $H$.
- Low leverage: outlying $y_{i}$ unlikely to impact our prediction equation
- High leverage: outlying $y_{i}$ can dramatically impact the prediction equation

```ad-important
The rule of thumb is that leverages **GREATER THAN** $2(p+1)/n$ are considered high.
```

While the tool of leverage helps us assess the impact **outliers** have on the overall regression line, we need to identify them in the first place.

---
### Detecting Outliers
How do we distinguish between truly unusual outcomes and large, but not unexpected, values for residuals? In particular, how do we identifying **outlying observations** in $y$ that may have **SMALL residuals** but **HIGH leverage**?

A good idea would be to **exclude** point $i$, and recompute $\hat{\beta}_{(i)}$ and $\hat{y}_{(i)}$