[[2024-09-19]] #Regression #R

### Prediction Intervals 
Motivation: We know our predicted values $\hat{y}$ are just compromises, best guesses based on some linear approximations. There’s still left over variability that we can’t account for.
- Exhibit: $R^{2}\le 0 \iff RSS > 0 \iff e^{T}e>0$ - residuals is non-zero
- Predicting at an uncertainty range of $[580,620]$ is much different than predicting $[400,800]$

These will be called **prediction intervals**.

We've learned from last lecture about forming confidence interval for conditional expectations: ![[7 Inferences#^cbe93c]]
However, this is not an appropriate behavior for **future observations** (predictions). 
- Consider the case $p=0$ and sample size $n \to \infty$: then $\overline{y}$ is going to have a confidence interval of $[\overline{y}, \overline{y}]$, which is very unlikely to capture future observation due to the error term $\epsilon_i$

Suppose we have collected $y_{1},\dots, y_{n}$ which are **iid** and **normally distributed** with $N(\mu_{y},\sigma^2)$ and $y^\star$ is a future realization from this distribution. Recall from previous lecture the definition of sample mean and RMSE of $y$: ![[7 Inferences#^5965d1]] ![[7 Inferences#^8e3c23]]
We are curious about the following statistics: ^0527d8
- $(y^{\star} - \mu_{y})/\sigma$
	- $\mathbb{E}\left (\frac{y^{\star} - \mu_{y}}{\sigma}\right)= \frac{\mathbb{E}(y^{\star}) - \mathbb{E}(\mu_y)}{\sigma} = \frac{\mu_{y}-\mu_{y}}{\sigma}=0$
	- $\text{Var}\left (\frac{y^{\star} - \mu_{y}}{\sigma}\right)= \frac{1}{\sigma^{2}} \text{Var}(y^{\star})=1$
	- $N(0,1)$ - standard normal
- $\overline{y}$
	- $\mathbb{E}(\overline{y})=\mathbb{E}(\frac{1}{n}\sum y_{i})=\frac{1}{n} \sum \mathbb{E}(y_{i})=\frac{1}{n} n\mu_{y}=\mu_{y}$
	- $\text{Var}(\bar{y})=\text{Var}\left (\frac{1}{n}\sum y_{i}\right)= \frac{1}{n^{2}} \sum \text{Var}(y_{i})= \frac{n\sigma^{2}}{n^{2}}=\frac{\sigma^{2}}{n}$
	- $N(\mu_{y},\frac{\sigma^{2}}{n})$
- $y^{\star}-\bar{y}$
	- $\mathbb{E}(y^{\star}-\bar{y})=0$
	- $\text{Var}(y^{\star}-\bar{y})=\text{Var}(y^{\star})+\text{Var}(\bar{y})=\sigma^{2}+\frac{\sigma^{2}}{n}=\sigma^{2}(1+ \frac{1}{n})$
		- Since $\text{Cov}(y^{\star}, \bar{y})=0$
	- $N(0,\sigma^{2}(1+ \frac{1}{n}))$
Since we don't know $\sigma^2$ (population parameter), we estimate it using $\hat{\sigma}$ (RMSE). Thus, we may estimate $$\text{stdev}(y^{\star}-\bar{y})=\sqrt{\hat{\sigma}^{2}\left(1+ \frac{1}{n}\right)}$$
Now, suppose the stronger linear model holds, and that we are interested in forming a **prediction interval** for a future observation $y^\star$ with covariate values $\tilde{x}$. $$y^{\star}=\beta_{0}+\beta_{1} \tilde{x}_{1}+\dots+\beta_{p}\tilde{x}_{p}+\epsilon^{\star}$$ and $$ \epsilon_{1},\dots,\epsilon_{n}, \epsilon^{\star} \sim N (0,\sigma_{\epsilon}^{2}) $$
Here, $$\mathbb{E}(y^\star)=\mathbb{E}(\tilde{x}^{T}\beta+\epsilon^{\star})=\tilde{x}^{T}\beta$$ and $$\text{Var}(y^{\star})=\sigma_{\epsilon}^2$$
Note that this is different from the [[7 Inferences#Inference for Conditional Expectations|distribution for conditional expectations]] we're seeing earlier as we have $y^\star$ as a (fixed) future observation that follows the **population distribution**, instead of relying on the predictor variables $X$.

#### Population Prediction Intervals
Then, we would construct a valid $100 (1-\alpha)\%$ prediction interval for a future observation at $\tilde{x}$: $$\tilde{x}^{T} \beta \pm z_{1-\alpha/2}\sigma_{\epsilon}$$
Recall the assumptions for stronger linear model again:  ![[4 Hypothesis Tests#^bfc3d4]]
Here, the **linearity** provides the **center** of our intervals. **Homoskedasticity** and **normality** provide the **width**.

```ad-example
**Example**: Prediction Intervals, $p=1$

![[Pasted image 20240920141010.png|400]]

If all three assumptions are met: At any point $\tilde{x}$, the population distribution of $y$ values for individuals whose $X$ variable equals $\tilde{x}$:
1. Has expectation of $\tilde{x}^{T}\beta$
2. Has a standard deviation of $\sigma_{\epsilon}$ (homoskedasticity)
3. Follows a normal distribution (normality)
```

#### Actionable Prediction Intervals
While valid, we can’t actually use this interval in practice:
- We don't know $\beta$ - crucial for the center of the interval
- We don't know $\sigma_{\epsilon}$ - crucial for the spread

We’ll now try to replace these components by sample-based estimators. This will change both the **center**, the **width**, and the **distribution** used to form the interval.

Again, suppose the stronger linear model holds, and that we are interested in forming a **prediction interval** for a future observation $y^\star$ with covariate values $\tilde{x}$. $$y^{\star}=\beta_{0}+\beta_{1} \tilde{x}_{1}+\dots+\beta_{p}\tilde{x}_{p}+\epsilon^{\star}$$ and $$ \epsilon_{1},\dots,\epsilon_{n}, \epsilon^{\star} \sim N (0,\sigma_{\epsilon}^{2}) $$
##### Center of Prediction Intervals
The first $n$ observations represent our observed data, from which we use to form coefficients estimates $\hat{\beta}$ and my estimated prediction equation $$\hat{\mu}_{y|\tilde{x}}=\hat{\mathbb{E}}(y|x=\tilde{x})=\tilde{x}^{T}\hat{\beta}$$
Note that $$\mathbb{E}(y^{\star}-\hat{\mu}_{y|\tilde{x}})=0$$ In words, the **expected value for a future response** at $\tilde{x}$ equals the expected value of my **SAMPLE-BASED prediction equation** when evaluated at $\tilde{x},\hat{\mu}_{y|\tilde{x}}$ . 

Hence, instead of centering our prediction interval at $\tilde{x}^{T}\beta$, we center it at $$\tilde{x}^{T}\hat{\beta}=\hat{\mu}_{y|\tilde{x}}$$

##### Variance for Prediction Intervals
When centering our prediction intervals at  $\hat{\mu}_{y|\tilde{x}}$, we need to take **two sources of variability** into account:
- $\text{Var}(y^{\star})$ (future observation)
-  $\text{Var}(\hat{\mu}_{y|\tilde{x}})$ (center of interval)

Observe that $y^{\star}$ and $\hat{\mu}_{y|\tilde{x}}$ are **INDEPENDENT of one another**.
- $\hat{\mu}_{y|\tilde{x}}$ is computed using **OBSERVED data**, with outcomes $y_{1}, \dots, y_n$
- $y^{\star}$ is a **FUTURE observation**, independent of $y_{1},\dots, y_{n}$, which was not used in estimating $\hat{\beta}$

Using the independence of $y^{\star}$ and $\hat{\mu}_{y|\tilde{x}}$ and our results on **[[7 Inferences#^99db9f|linear combinations of slopes]]**, we can derive a variance to help determine the width of our prediction intervals: $$\begin{align}\text{Var}(y^{\star}-\hat{\mu}_{y|\tilde{x}})&=\text{Var}(y^{\star})+\text{Var}(\hat{\mu}_{y|\tilde{x}}) \\ &= \sigma_{\epsilon}^{2}+\sigma_{\epsilon}^{2} \tilde{x}^{T}(X^{T}X)^{-1}\tilde{x} \\ &= \sigma_{\epsilon}^{2}(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})\end{align}$$
Drawing conclusion back [[#^0527d8|here]], we have that $$\frac{y^{\star}-\hat{\mu}_{y|\tilde{x}}}{\sigma_{\epsilon} \sqrt{(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})}} \sim N(0,1)$$
Since, again, the value of $\sigma_{\epsilon}$ is unknown to us, so we will use estimate it using RMSE: $$\hat{\sigma}_{\epsilon}= \sqrt{\frac{1}{n-p-1} \sum_{i=1}^{n} e^{2}}$$ which is the standard deviation of the residuals.

Putting it all together, $$T=\frac{y^{\star}-\hat{\mu}_{y|\tilde{x}}}{\hat{\sigma}_{\epsilon} \sqrt{(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})}}  \sim t_{n-p-1}$$ under the stronger linear model. Based on this statistic, an $100 (1-\alpha)\%$ prediction interval for a covariate $\tilde{x}$ is $$\hat{\mu}_{y|\tilde{x}} \pm t_{1-\alpha/2,n-p-1}\hat{\sigma}_{\epsilon} \sqrt{(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})}$$ where $\hat{\sigma}_{\epsilon} \sqrt{(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})}$ is the standard error for the predicted response $\hat{\mu}_{y|\tilde{x}}$.

Note the similarity and difference to a [[7 Inferences#^cbe93c|confidence interval for conditional expectation]].

##### Homoskedasticity
Homoskedasticity is crucial for the construction of prediction intervals. It allows us to pool information **across residuals** (formed at different $\tilde{x}$) to calculate an estimate of the standard deviation of $y$ any point $\tilde{x}$.
- $\hat{\text{Var}}(y^{\star}|\tilde{x})=\hat{\sigma}_{\epsilon}^{2}$

If the distribution of $y$ is in fact heteroskedastic, however, then 

![[Pasted image 20240921142230.png|400]]

The $\hat{\sigma}_{\epsilon}$ we computed **may NOT** be a good estimate of the true standard deviation $\sigma_{\epsilon}$, as in this example $\hat{\sigma}_{\epsilon}$ may be too large for smaller $y$ values and vice versa as we assumed constant variances.

##### Approximation of Prediction Interval
An approximation to the prediction intervals produced by `R` (appropriate when $n$ is **large**) is given by $$\hat{\mu}_{y|\tilde{x}} \pm z_{1-\alpha/2}\hat{\sigma}_{\epsilon}$$ which ignores the **randomness** in $\hat{\sigma}_{\epsilon}$ and randomness in $\tilde{x}^{T}\hat{\beta}$.

When $n \to \infty$, $\hat{\sigma}_{\epsilon} \to \sigma_{\epsilon}$ and randomness in the center of interval is **small** relative to $\sigma_{\epsilon}$.
- If all we have is the output from `lm`, this would be a reasonable approximation, otherwise use `predict`
- Exactly what we've shown in [[#Population Prediction Intervals|population prediction interval]], except for the usage of RMSE (estimator for standard deviation) instead of true population standard deviation

```ad-note
**Empirical Rules for Predictions**
- $50\%$ of future responses will fall within $\pm 2/3\hat{\sigma}_{\epsilon}$ from their predicted value
- $68\%$ of future responses will fall within $\pm 1\hat{\sigma}_{\epsilon}$ from their predicted value
- $95\%$ of future responses will fall within $\pm 2\hat{\sigma}_{\epsilon}$ from their predicted value 
- $99.7\%$ of future responses will fall within $\pm 3\hat{\sigma}_{\epsilon}$ from their predicted value
```

---
### Comparison: Prediction Intervals and Confidence Intervals
We’ve recently introduced (1) **prediction intervals** for future observations $y^\star$ at a point $\tilde{x}$; (2) and **confidence intervals** for predicted mean response $\mathbb{E}(y|\tilde{x})={\mu}_{y|\tilde{x}}$.

```ad-important
**Confidence Intervals and Prediction Intervals**

(1) **Prediction Interval** for future observations $y^\star$: $$\hat{\mu}_{y|\tilde{x}} \pm t_{1-\alpha/2,n-p-1}\hat{\sigma}_{\epsilon} \sqrt{(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})}$$
(2) **Confidence Interval** for ${\mu}_{y|\tilde{x}}$: $$\hat{\mu}_{y|\tilde{x}} \pm t_{1-\alpha/2,n-p-1}\hat{\sigma_{\epsilon}}\sqrt{\tilde{x}^{T}(X^TX)^{-1}\tilde{x}}$$

Both have the **same center**, but **different width.** Prediction intervals are wider (often much wider) than confidence intervals.

**Observations**
- Variance of $\hat{\mu}_{y|\tilde{x}}$, $\sigma_{\epsilon}^{2} \tilde{x}^{T}(X^{T}X)^{-1}\tilde{x}$ tends to **zero** as $n \to \infty$
- Variance used for **prediction intervals**, $\sigma_{\epsilon}^{2}(1+\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})$, tends to **one** as $n \to \infty$
	- This is effectively saying that, even if we knew $\beta$ (and hence $\mu_{y|\tilde{x}}$ and hence my confidence interval for predicted mean response is just a point), we still **CAN'T** perfectly predict future observations, given the variances introduced by the error $\epsilon_i$

![[Pasted image 20240921160644.png|400]]

Note the confidence intervals are substantially **narrower** than the prediction intervals.
```

The two types of intervals address **different questions**.
- Prediction interval addresses the **uncertainty** of an **INDIVIDUAL future response** given some predictor value
- Confidence interval addresses the **uncertainty** of **AVERAGE predicted response** given some predictor value

[See further reading here](https://stats.stackexchange.com/a/271232). Also [here](https://piazza.com/class/ly4q6d2b6vz208/post/115).

```ad-note
For any type of interval estimation, formally, we use the **expected value** of our variable of interest as the **center of the interval** (which is often an estimator of the variable of interest), and the **standard error** of the **difference between the true value of the variable of interest and its estimator** as the **width of the interval**.
- Note that for all CI calculations we have conducted, the standard error term is solely determined upon the **variance of the estimator** since the true population parameter is a constant, which is **NOT the case** for PI
```

---
### Covered `R` Functions

```r
xnew = data.frame(income = 75, competitors = 3)

# Prediction Intervals
predict(lm.both, newdata = xnew, interval ="prediction", level = .95)
>>> 578.1509 441.0814 715.2204

# Confidence Intervals
predict(lm.both, newdata = xnew, interval = "confidence", level = .95)
>>> 578.1509 553.3363 602.9655
```
