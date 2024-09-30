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
- $e_{i}=\hat{y}_{i}-$