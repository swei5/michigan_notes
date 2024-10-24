[[2024-10-22]] #Regression 

*What happens if certain LR assumptions seem unreasonable? Can we proceed? If so, how?*
- We addressed concerns with heteroskedasticity in [[13 Heteroskedasticity|Lecture 13]]
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
While asymptotic normality provides an assurance that things won’t go too far off the rails when $n$ is large enough, we generally don’t know how **suitable** the approximation will be for any particular data set.

Can we try to use the data itself to approximate the distribution of $t_{\text{stat}}$, rather than relying on the central limit theorem?

```ad-example
**Example**: Bootstrap Example Without Covariates

We’ll now build intuition for the bootstrap through a simple example without covariates (we will consider inference for the population mean). Suppose that $y_{i},i=1,\cdots,50$ are iid draws from the following discrete distribution.

![[Pasted image 20241023010252.png|500]]

Issue: **right-skewness** in $y_{i}$ affects the distribution of $\bar{y}$ for even moderate $n$.

For regression with an intercept only, we base our confidence interval on multipliers from the $t_{n-1}$ distribution. The validity of these intervals hinges upon the fact that, if $\bar{y}$ is *close to* **normally distributed**: $$\mathbb{P}\left(t_{0.025,n-1} \le \frac{\bar{y}-\mu}{\text{se}(y)} \le t_{0.975, n-1}\right)\approx 0.95$$

When we make a $t_{n−1}$ interval, we’re saying that the $t_{n−1}$ can provide approximations to the $0.025$ and $0.975$ quantiles for $t_{\text{stat}}=\frac{\bar{y}-\mu}{\text{stdev}(y)/\sqrt{n}}$.

![[Pasted image 20241023010953.png|500]]

However, note that because of the skewness of the data, the actual distribution of $t_{\text{stat}}$ deviates from the actual $t$ distribution.

What if we could replace our intervals based on the $t$ distribution with intervals based on the actual distribution of $t_{\text{stat}}$ in the example at hand: $$\left[\bar{y}-q_{0.975}(t_{\text{stat}}) \frac{\text{stdev}(y)}{\sqrt{n}}, \bar{y}-q_{0.025}(t_{\text{stat}}) \frac{\text{stdev}(y)}{\sqrt{n}}\right]$$

where $q_{0.975}(t_{\text{stat}})$ and $q_{0.025}(t_{\text{stat}})$ are quantiles from the **true distribution** of $t_{\text{stat}}$.
```

```ad-important
**Definition 14.2**: Ideal Confidence Intervals

If we had access to the true distribution for $y_i$, $F_{y}$ , we could construct a confidence interval by doing the following $B$ times.
1. Take $n$ iid samples from the distribution of $y_i$, $F_{y}$. Store the resulting sample as $\{y_{1}^{\star},\cdots,y_{n}^{\star}\}$
2. Find $\bar{y}^{\star}$ and $\text{stdev}(y^{\star})$
3. Form $t^{\star}_{\text{stat}}=\frac{\bar{y}^{\star}-\mu}{\text{stdev}(y^{\star})/\sqrt{n}}$

After this is done, find the quantiles $q_{0.975}(t^{\star}_{\text{stat}})$ and $q_{0.025}(t^{\star}_{\text{stat}})$ based on $B$ simulated values, and use these to form confidence intervals. The coverage would be $0.95$, in this case.

This is called a **Monte-Carlo approximation** to the quantiles of $t_{\text{stat}}$.
```

^6a8a9a

#### Plug-in Bootstrap
While these intervals would be great, these **aren’t actionable** since we don’t know the true distribution $t_{\text{stat}}$ because we don't know the population distribution of $y_{i}$.
- If we knew the true distribution of $y_{i}$, ironically, we'd know $\mu$ as a consequence and we shouldn't need to conduct any inference

We can estimate it too, however.

In practice, $F_{y}$, the true distribution function, is unknown to us. An intuitive way to go around this would be to use the proportions in our data sample.

![[Pasted image 20241023012304.png|400]]

```ad-important
**Definition 14.3**: Empirical Distribution

The **empirical distribution function** $\hat{F}_{y}$ based upon a sample $y=\{y_{1},\cdots, y_{n}\}$ evaluated at a point $c$ counts how many observations were **at or below** that $c$: $$\begin{align}
\hat{F}_{y}(c) &= \text{Prop}(y_{i}\le c)\\
&= \frac{1}{n} \sum\limits_{i=1}^{n} \mathbb{1}(y_{i}\le c)
\end{align}$$

If $y_{1},\cdots, y_{n}$ are iid from a distribution with a cumulative distribution function $F_{y}$, $\hat{F}_{y}$ estimates $F_{y}$: $$\hat{F}_{y}(c) \xrightarrow[]{p} F_{y}(c)$$ as $n \to \infty$.
```

A **bootstrap sample** from a data set is an iid sample of size $n$ from $\hat{F}_{y}$, the empirical distribution of $y$.
- We treat the observed distribution in our data set as though it were the true distribution, and we take an iid sample from this distribution

For any type of variable (continuous or discrete), I can draw an iid sample of size $n$ from $\hat{F}_{y}$ by running `yboot = sample(y, n, replace = T)` in `R`.
- That is, sampling $n$ times **with replacement** from a vector $y$ gives an iid sample from the empirical distribution of $y$

Let $\{y_{1}^{\star},\cdots,y_{n}^{\star}\}$ denote the resulting iid sample of size $n$ from $\hat{F}_{y}$. Across the bootstrap resamples 
- $\hat{F}_{y}$ becomes the true **population distribution** for $y_{i}^{\star}$
- Any **population parameter** within the bootstrap framework is derived from $\hat{F}_{y}$

```ad-note
In real life, in my single sample $y_{1},\cdots, y_{n}$
- $\hat{F}_{y}$ estimates $F_{y}$
- $\bar{y}$ estimates $\mu$
- $\text{stdev}(y)$ estimates $\sigma$

If I then take a bootstrap sample of $y_{1},\cdots, y_{n}$; for any single bootstrap sample $y_{1}^{\star},\cdots,y_{n}^{\star}$
- The empirical distribution of $y_{i}^{\star}$, $\hat{F}_{y^{\star}}$, estimates $\hat{F}_{y}$
- $\bar{y}^{\star}$ estimates $\bar{y}$
- $\text{stdev}(y^{\star})$ estimates $\text{stdev}(y)$

Most importantly, we **CANNOT** generate multiple data sets with distribution $F_{y}$ (as we don't know $F_{y}$), whereas we **CAN** with the empirical distribution $\hat{F}_{y}$.
```

With the bootstrap technique, we can reform our $t_{\text{stat}}$ based on values $y_{1}^{\star},\cdots,y_{n}^{\star}$$$t^{\star}_{\text{stat}}=\frac{\bar{y}^{\star}-\bar{y}}{\text{stdev}(y^{\star})/\sqrt{n}}$$ The benefit of this is that unlike $F_{y}$, we have access to $\hat{F}_{y}$. It is defined based on our sample!
- We can generate multiple data sets according to the distribution $\hat{F}_{y}$
- We can then actually calculate the quantiles for $t^{\star}_{\text{stat}}$

Thus, we can redesign everything formed in [[#^6a8a9a|Definition 14.2]]. 

```ad-important
**Definition 14.4**: The Bootstrap for $t$ Statistics

For a given data set $\{y_{1},\cdots, y_{n}\}$ with observed mean $\bar{y}$, we could construct a confidence interval by doing the following $B$ times.
1. Sample $n$ times **with replacement** from the values $\{y_{1},\cdots, y_{n}\}$. Store the resulting sample as $\{y_{1}^{\star},\cdots,y_{n}^{\star}\}$
2. Find $\bar{y}^{\star}$ and $\text{stdev}(y^{\star})$
3. Form $t^{\star}_{\text{stat},b}=\frac{\bar{y}^{\star}-\bar{y}}{\text{stdev}(y^{\star})/\sqrt{n}}$

After this is done, find the quantiles $q_{0.975}(t^{\star}_{\text{stat}})$ and $q_{0.025}(t^{\star}_{\text{stat}})$ based on $B$ simulated values, and use these to form confidence intervals. The coverage would be $0.95$, in this case.
```

In general, we are trying to approximate the distribution of $$t_{\text{stat}}=\frac{\bar{y}-\mu}{\text{stdev}(y)/\sqrt{n}}$$ by that of $$t^{\star}_{\text{stat}}=\frac{\bar{y}^{\star}-\bar{y}}{\text{stdev}(y^{\star})/\sqrt{n}}$$ instead of simply asserting that it is $t$ -distributed, like we did before.

```ad-note
**Bootstrap**

Rather than employing quantiles based on the $t$-distribution, we use **simulation** to try to estimate these quantiles.
- Can be shown to work under very mild conditions
- Works with continuous or discrete variables
- Can also be used to create confidence intervals for quantities other than the population mean
```

#### Bootstrapping in Regression
We will now apply the same principles to better approximating the distributions of $$\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}_{HC2}(\hat{\beta}_{j})}$$ or $$\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}(\hat{\beta}_{j})}$$
In analogy to what we saw when bootstrapping the mean, in the bootstrap world:
- The **true** population slope coefficients will be $\hat{\beta}$
- Each bootstrap sample will generate a value $\hat{\beta}^\star$, an estimate of $\hat{\beta}$
- Each bootstrap sample will also generate a **standard error estimator**
- Each bootstrap sample will generate a value for $t^{\star}_{\text{stat}}$

The distribution of $t^{\star}_{\text{stat}}$ across simulations will serve as our estimate for the distribution $t_{\text{stat}}$, rather than using the $t_{n-p-1}$ distribution.

There are two prevalent bootstrap schema, depending upon whether one is willing to assume homoskedasticity.
1. **Residual Bootstrap** (assumes homoskedasticity): $\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}(\hat{\beta}_{j})}$
2. **Pairs Bootstrap** (valid even if heteroskedastic): $\frac{\hat{\beta}_{j}-\beta_{j}}{\text{se}_{HC2}(\hat{\beta}_{j})}$

```ad-important
**Definition 14.5**: Residual Bootstrap

Let $\hat{\beta}$ be the OLS coefficients from a regression of $y$ on $X$. For $b=1,\cdots, B$ bootstrap samples:
1. Sample $n$ times **with replacement** from the values $\{e_{1},\cdots, e_{n}\}$. Store the resulting sample as $\{e_{1}^{\star},\cdots,e_{n}^{\star}\}$
2. Construct $y_{i}^{\star}=x_{i}^{T}\hat{\beta}+e_{i}^{\star}$ for $i=1,\cdots,n$
3. Run regression of $y^{\star}$ on $X$ - call the result `lm*`
4. Calculate $\hat{\beta}_{j}^\star$ and $\text{se}(\hat{\beta}_{j}^\star)$ using the regresion `lm*`
5. Form $t_{\text{stat,b}}^{\star}$ as $$t_{\text{stat,b}}^{\star}=\frac{\hat{\beta}_{j}^{\star}-\hat{\beta}_{j}}{\text{se}(\hat{\beta}_{j}^{\star})}$$

And we use quantiles of $t_{\text{stat,b}}^{\star}$ to construct confidence intervals.
```

If **homoskedasticity** appears reasonable, the residual bootstrap is generally **preferred**.
- Treats $X$ as fixed, induces randomness through resampling residuals
- More aligned with the data generating mechanism we’ve considered
- Performs better in small samples (more stable) than the pairs bootstrap

```ad-important
**Definition 14.6**: Residual Bootstrap

Let $\hat{\beta}$ be the OLS coefficients from a regression of $y$ on $X$. For $b=1,\cdots, B$ bootstrap samples:
1. Sample $n$ times **with replacement** from the values pairs $\{(y_{1},x_{1}),\cdots, (y_{n},x_{n})\}$. Store the resulting sample as $\{(y_{1}^{\star},x_{1}^{\star}),\cdots,(y_{n}^{\star},x_{n}^{\star})\}$
2. Construct $y_{i}^{\star}=x_{i}^{T}\hat{\beta}+e_{i}^{\star}$ for $i=1,\cdots,n$ and $X^{\star}$, the $n \times (p+1)$ matrix whose $i$th row contains $x^{\star}_{i}$
3. Run regression of $y^{\star}$ on $X^{\star}$ - call the result `lm*`
4. Calculate $\hat{\beta}_{j}^\star$ and $\text{se}_{HC2}(\hat{\beta}_{j}^\star)$ using the regresion `lm*`
5. Form $t_{\text{stat,b}}^{\star}$ as $$t_{\text{stat,b}}^{\star}=\frac{\hat{\beta}_{j}^{\star}-\hat{\beta}_{j}}{\text{se}_{HC2}(\hat{\beta}_{j}^{\star})}$$

And we use quantiles of $t_{\text{stat,b}}^{\star}$ to construct confidence intervals.
```

Under **heteroskedasticity**, the pairs bootstrap is our **ONLY** option
- Breaks the association between $e_{i}$ and $x_{i}$ - reasonable under homoskedasticity, but not under heteroskedasticity
- Residual bootstrap doesn’t provide valid inference under heteroskedasticity
- Pairs bootstrap maintains correspondence between pairs $(y_{i},x_{i})$, and hence preserves heteroskedasticity in the bootstrap world
