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

Suppose we compared two individuals who differ in their $x$ values by $1$ percent. What we we predict the difference in their predicted $y$ values to be? $$\begin{align}
\hat{y}_{1}-\hat{y}_{2}&=\hat{\beta}_{1} (\ln(1.01x)-\ln(x))\\
&= \hat{\beta}_{1} \ln(1.01)
\end{align}$$
```ad-important
**Definition 15.1**: Approximate Interpretation under Logarithmic Growth

Two observations who differ in $x$ by $\inc$
```
