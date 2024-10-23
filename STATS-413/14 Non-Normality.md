[[2024-10-22]] #Regression 

*What happens if certain assumptions seem unreasonable? Can we proceed? If so, how?*
- We address concerns with heteroskedasticity in [[13 Heteroskedasticity|Lecture 13]]
- Today, we will focus on violations of normality

---
### Non-Normality
We’ll now assess the impact of departures from normality on inference. Namely, under the stronger linear model, we obtained the result that $$t_{\text{stat}}=\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}(\hat{\beta}_{j})} \sim t_{n-p-1}$$
However, when $\varepsilon$ isn’t normally distributed, we can’t guarantee that $\beta_{j}$ is normally distributed for all sample sizes $n$.
- Prone to Type I errors
- Confidence intervals using quantiles from $t_{n-p-1}$ might have larger or smaller actual converge

Though, it is less of a concern when $n$ is large. Recall the **central limit theorem** states that if $y_{1},\cdots, y_{n}$ are **iid** then regardless of the distribution of $y_{i}$, as $n \to \infty$, $$\frac{\bar{y}-\mu}{\sigma/\sqrt{n}}\xrightarrow[]{d} N(0,1)$$
Now, consider our model for heteroskedasticity: ![[13 Heteroskedasticity#^e387c1]]
Let $\hat{\beta}$ be the OLS slope coefficients, and recall that $$\text{Var}(\hat{\beta})=\sigma_{\varepsilon}^{2}(X^{T}X)^{-1}X^T W^{-1}X(X^{T}X)^{-1}$$
We will now consider the distribution of
1. $\frac{\hat{\beta}_{j}-\beta_{j}}{\text{stdev}(\hat{\beta}_{j})}$
2. $\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}_{HC2}(\hat{\beta}_{j})}$
3. $\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}(\hat{\beta}_{j})}$

where $\text{stdev}(\cdot)=\sqrt{\text{Var}(\cdot)}$ , $\text{se}(\cdot)$ is the conventional standard error returned by `R` and also [[4 Hypothesis Tests#^bde6fa|here]] and $\text{se}_{HC2}(\cdot)$  is the HC2 standard error.

```ad-important
**Definition 14.1**: Central Limit Theorems for Regression Coefficients

Under our model for heteroskedasticity and under suitable conditions on $\varepsilon$ and $X$, as $n \to \infty$: $$\frac{\hat{\beta}_{j}-\beta_{j}}{\text{stdev}(\hat{\beta}_{j})} \xrightarrow[]{d} N(0,1)$$ $$\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}_{HC2}(\hat{\beta}_{j})} \xrightarrow[]{d} N(0,1)$$
If homoskedasticity holds ($W=I$), then we further have that as $n \to \infty$, $$\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}(\hat{\beta}_{j})} \xrightarrow[]{d} N(0,1)$$
```

Under heteroskedasticity, $\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}(\hat{\beta}_{j})}$ will still be **asymptotically normally distributed** with mean zero, but with a variance that **does NOT equal** $1$.

```ad-note
Because $\hat{\beta}_{j}$ is asymptotically normal, confidence intervals and hypothesis tests using the $t$ distribution will also be **asymptotically valid**.
- Under homoskedasticity: we can use the usual standard errors from `R`
- Under heteroskedasticity: need to replace the usual standard errors with HC standard errors

In practice: for $n$ reasonably large, methods for inference using the $t$ will be **roughly correct** so long as we use a **valid standard error**. In other words,
- Confidence intervals will have coverage close to $100(1-\alpha)\%$ for *large enough* $n$
- Hypothesis tests will have Type I error rate close to $\alpha$ for *large enough* $n$

The extent depends on the distribution of $\varepsilon$. The more skewed the distribution, the larger $n$ will need to be and vice versa.
```

---
### Bootstrap

