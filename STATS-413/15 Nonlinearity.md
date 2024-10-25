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
Let $y=\{y_{1},\cdots, y_{n}\}$ be positive. Then,

$$\text{mean}(\log(y)) \ne \log(\text{mean}(y))$$
$$\text{stdev}(\log(y)) \ne \log(\text{stdev}(y))$$

Let $Z$ be a positive random variable
```

