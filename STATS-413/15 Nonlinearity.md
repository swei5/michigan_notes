[[2024-10-24]] #Regression

*What happens if certain LR assumptions seem unreasonable? Can we proceed? If so, how?*
- We addressed concerns with heteroskedasticity in [[13 Heteroskedasticity|Lecture 13]]
- We addressed violations of normality in [[14 Non-Normality|Lecture 14]]
- Today we will deal with nonlinearity

If the conditional expectation is nonlinear in the predictors, but we nonetheless conduct an ordinary least squares regression of $y$ on $X$, $$\mathbb{E}(\tilde{x}^{T}\beta)\ne\mathbb{E}(y|\tilde{x})$$ and $$\mathbb{E}(e_{i}|x_{i})\ne 0$$
In the case when the true conditional expectation is nonlinear, we may
- Consider transformations of the response variable
- Consider transformations of the predictor variables

These fall under two main approaches:
1. Transformations driven by intuition / understanding of the **data generating process**
	- We might still try to interpret slope coefficients after transformation
2. Transformations affording **flexibility** to approximate nonlinear truth
	- Here, our main focus will be on the **accuracy** of predictions made using our model

---
### Log Transformation
Perhaps the most commonly applied transformation of predictors and/or response variables is the log transformation.
- Variable needs to take on strictly **positive** value
- Commonly used with financial data
- In general, used for variables whose distributions tend to be **right skewed**

It is useful when changes in a particular variable occur on a multiplicative / **percentage scale**, rather than an **additive scale**.

```ad-note
Let $y=\{y_{1},\cdots, y_{n}\}$ be positive. Then, $$\text{mean}(\log(y)) \ne \log(\text{mean}(y))$$ $$\text{stdev}(\log(y)) \ne \log(\text{stdev}(y))$$

Let $Z$ be a positive random variable, then $$\mathbb{E}(\log(Z)) \ne \log(\mathbb{E}(Z))$$
```

```ad-example
**Example**: Logs and Percent Changes

Suppose that the difference in salaries between two individuals is $10\%$: $$x_{1}/x_{2}=1.1$$ Applying logs to both sides: $\log(x_{1}/x_{2})=\log(x_{1})-\log(x_{2})=\log(1.1)$

Differences that were **multiplicative** before become **additive** after taking a log transform.

In linear regression, slope coefficients relate **additive differences** in predictor values to **additive differences in responses**.
- Diminishing returns
```

We will only use base- $e$ logarithms when transforming variables for regression analysis.

#### Logarithmic Growth
If **percentage differences** in $x$ relate to **additive differences** in $y$ , we could consider the following generative model for logarithmic growth: $$y_{i}=\beta_{0}+\beta_{1} \ln(x_{i})+\varepsilon_{i}$$ If we define the transformed variable $l_{i}=\ln(x_{i})$, we would have a linear model on the transformed scale: $$y_{i}=\beta_{0}+\beta_{1} l_{i}+\varepsilon_{i}$$
```ad-example
**Example**: The Log in Action

The linear approximation wasn’t terrible, but the trend is much better approximated by the log transform.

![[Pasted image 20241025132304.png|400]]
```

Suppose we compared two individuals who differ in their $x$ values by $1$ percent. If the two units differed by $\Delta \%$ , the expected difference in  $y$ would be a factor of $\beta_{1} \ln(1+\Delta/100)$: $$\begin{align}
\hat{y}_{1}-\hat{y}_{2}&=\hat{\beta}_{1} (\ln((1+\Delta\%)x)-\ln(x))\\
&= \hat{\beta}_{1} \ln(1+\Delta\%)\\
&\approx \hat{\beta}_{1}(\Delta/100)
\end{align}$$
In other words, two observations who differ in $x$ by $\Delta \%$ are predicted to differ in $y$ by **roughly** $\hat{\beta}_{1}(\Delta/100)$ (provided $|\Delta| < 10\%$).

![[Pasted image 20241025174739.png|300]]

#### Exponential Growth
In a system undergoing exponential growth as a function of $x$, one may observe that additive differences in $x$ correspond to percentage (multiplicative) differences in $y$ . This is consistent with the model $$y_{i}=\exp({\beta_{0}+\beta_{1}x_{i}+\varepsilon_{i}})$$ If we take the log transformation of the response, we’d return to a linear model $$\ln(y_{i})=\beta_{0}+\beta_{1}x_{i}+\varepsilon_{i}$$ from whence we can run a linear regression of $\ln(y_{i})$ on $x_{i}$ to estimate $\beta$. 

Under exponential growth, consider two units who differ in $x$ by one unit: $$\begin{align}
\ln(\hat{y}_{1})-\ln(\hat{y}_{2})&=\hat{\beta}_{1}\\
\ln(\hat{y}_{1}/\hat{y}_{2})&= \hat{\beta}_{1}\\\\
\frac{\hat{y}_{1}}{\hat{y}_{2}}&= \exp(\hat{\beta}_{1})\\
&\approx (1+\hat{\beta}_{1}\Delta)
\end{align}$$ That is, two units who differ in $x$ by one unit differ in their predicted values for $y$ by a factor of $\exp(\hat{\beta}_{1})$. If they differ by $\Delta$ units, their predictions would differ by a factor of $\exp(\Delta \hat{\beta}_{1})$.

In other words, two observations who differ in $x$ by $\Delta$ units are predicted to differ in $y$ by **roughly** $(100\hat{\beta}_{1})\Delta \%$ (provided $|\hat{\beta}_{1}\Delta| < 0.1$).

#### Log-Log Transformation
In other applications, relationships may be best thought of as percentage (multiplicative) changes in $x$ being predictive of percentage (multiplicative) changes in $y$.

```ad-example
**Example**: Elasticity

In this case, we’d anticipate the following functional form: $$y_{i}=x_{i}^{\beta_{1}} \exp(\beta_{0}+\varepsilon_{i})$$ Taking logs of both sides, $$\ln(y_{i})=\beta_{0}+\beta_{1}\ln(x_{i})+\varepsilon_{i}$$ so that after log transformation of $x$ and $y$ we have a **linear** model. We can now run a linear regression of $\ln(y)$ on $\ln(x)$.

![[Pasted image 20241025203330.png|400]]
```

Suppose two units differ in $x$ by $\Delta\%$. $$\begin{align}
\ln(\hat{y}_{1})-\ln(\hat{y}_{2})&=\hat{\beta}_{1}(\ln((1+\Delta \%)x)-\ln(x))\\
\ln(\hat{y}_{1}/\hat{y}_{2})&=\hat{\beta}_{1}\ln(1+\Delta \%)\\
\ln\left(1+\frac{\hat{y}_{1}-\hat{y}_{2}}{\hat{y}_{1}}\right)&= \hat{\beta}_{1}\ln(1+\Delta \%)\\
\frac{\hat{y}_{1}}{\hat{y}_{2}}&= (1+\Delta/100)^{\hat{\beta}_{1}}\\
\frac{\hat{y}_{1}-\hat{y}_{2}}{\hat{y}_{1}}&\approx (\hat{\beta}_{1}\Delta)/100
\end{align}$$ If they differ by $\Delta \%$ in $x$, we'd expect them to differ in $y$ by a factor of $(1+\Delta/100)^{\hat{\beta}_1}$.  Approximately, two observations who differ in $x$ by $\Delta \%$ are predicted to differ in $y$ by $\hat{\beta}_{1}\Delta \%$ (provided $|\hat{\beta}_{1}\Delta|<10\%$, $|\Delta| < 10\%$).

---
### Prediction Intervals
Suppose that we’ve run a linear regression using $\ln(y)$ as a response variable, and that we’d like to form a $95\%$ prediction interval for $y$ at a given value for the predictors $\tilde{x}$. We could certainly construct prediction intervals for $\ln(y)$ if the **stronger linear model** held: linearity, homoskedasticity, normality.

A $100 (1-\alpha)\%$ prediction interval for $\ln(y)$ takes the form $$\tilde{x}^{T}\hat{\beta} \pm t_{1-\alpha/2,n-p-1}\hat{\sigma}_{\varepsilon} \sqrt{(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})}$$ we can use this to directly get a prediction interval for $y$ exponentiating the endpoints, that is $$[\exp(\text{lb}), \exp(\text{ub})]$$ for lower bounds and upper bounds of $100 (1-\alpha)\%$ prediction interval for $\ln(y)$.

```ad-important
**Definition 15.1**: Quantiles and Logrithms

Let $Z$ be any (positive) variable, and let $q_{p}(\cdot)$ be the $p$th quantile/percentile of a distribution. Then, $$q_{p}(\ln(Z))=\ln(q_{p}(Z))$$

This includes the median.
```

This is because the logarithm is a **monotone increasing transformation**. This means it preserves order!

![[Pasted image 20241029145428.png|400]]

Because the log is a monotone transformation, we know that for $y>0$: $$\text{lb} \le \ln(y) \le \text{ub} \iff \exp(\text{lb}) \le y \le \exp(\text{ub})$$