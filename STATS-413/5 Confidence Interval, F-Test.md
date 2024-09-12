[[2024-09-10]] #Regression #R

Recall some key properties of our linear model: ![[4 Hypothesis Testing#^bfc3d4]]
And that $$\mathbb{E}(\hat{\beta})=\beta$$
$$\text{Var}(\hat{\beta})=\sigma_{\epsilon}^{2}(X^{T}X)^{-1}$$ 
We thus have that
![[4 Hypothesis Testing#^e2a176]]

However at times when the population standard deviation of $\epsilon$ is unknown, we need to rely on the $t$ -test. We would need RMSE to compute the standard error: $$\hat{\sigma_{\epsilon}}=\sqrt{\frac{\sum_{i=1}^{n}e_{i}^{2}}{n-p-1}}$$$$\text{se}(\hat{\beta}_{j})=\hat{\sigma_{\epsilon}}\sqrt{(X^TX)^{-1}_{(j+1),(j+1)}}$$ And finally $$t_{\text{stat}}=\frac{\hat{\beta}_{j}^{\text{obs}}-\gamma_{0}}{\text{se}(\hat{\beta}_{j})}$$
### $p$ -Value Computation
Let $T_{n-p-1}$ be a random variable which follows a $t_{n-p-1}$ distribution:
- Less than alternative (`pt(tstat, n-p-1)`: $$p=\mathbb{P}(T_{n-p-1} \le t_{\text{stat}})$$
- Greater than alternative `1-pt(tstat, n-p-1)`: $$p=\mathbb{P}(T_{n-p-1} \ge t_{\text{stat}})=1-\mathbb{P}(T_{n-p-1} \le t_{\text{stat}})$$
- Two-sided alternative: $$p=\mathbb{P}(T_{n-p-1} \le |t_{\text{stat}}|)=2\mathbb{P}(T_{n-p-1} \ge t_{\text{stat}})=2\mathbb{P}(T_{n-p-1} \le t_{\text{stat}})$$
Using the fact that $t$ distribution is symmetric.

Let $t_{1-\frac{\alpha}{2},n-p-1}$ represent the $(1-\frac{\alpha}{2})$ quantile from a $t$ distribution with $n-p-1$ degrees of freedom. By symmetry: $$t_{1-\frac{\alpha}{2},n-p-1}=-t_{\frac{\alpha}{2},n-p-1}$$
Hence, note that $$\begin{align}1-\alpha&=\mathbb{P}\left(-t_{1-\frac{\alpha}{2},n-p-1} \le \frac{\hat{\beta}_{j}-\beta{j}}{\text{se}(\hat{\beta}_{j})} \le t_{1-\frac{\alpha}{2},n-p-1}\right) \\&= \mathbb{P}\left(\hat{\beta_j}-t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j}) \le \beta_{j} \le \hat{\beta_j}+t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j}) \right)
\end{align}$$
---
### Confidence Intervals for ${\beta}_{j}$
Provided the assumptions for the (stronger) linear model hold (linearity, homoskedasticity, normality of $\epsilon$), we may define the following.

```ad-important
**Definition 5.1**: Two-Sided Confidence Intervals for $\beta_{j}$

A $100(1 − \alpha)\%$ t-based two-sided confidence interval for $\beta_{j}$ is:
$$\hat{\beta_{j}}\pm t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j})$$

```

The corresponding `R` command for $t_{1-\frac{\alpha}{2},n-p-1}$ is `qt(1-a/2, n-p-1)`. To compute the entire confidence interval (both tails), we use `beta+c(-1,1)*qt(1-a/2,n-p-1)*se`

Or, `R` provides an easier shorthand - `confint(lm, level = .95)` for calculation of 95% CI with the `lm` regression model.

#### Interpretation of Confidence Interval 
If our confidence interval is $\hat{\beta_{j}}\pm t_{0.975, n-p-1} \text{se}(\hat{\beta}_{j})$, we say that we are 95% **confident** that $\beta_{j}$, the **true population slope**, falls between $\hat{\beta_{j}} + t_{0.975, n-p-1} \text{se}(\hat{\beta}_{j})$ and $\hat{\beta_{j}} - t_{0.975, n-p-1} \text{se}(\hat{\beta}_{j})$.
- In other words, values for $\beta_{j}$ outside the interval are **unlikely**

The interval is **random** since both $\hat{\beta_{j}}$ and $\text{se}(\hat{\beta}_{j})$ are random due to randomness of $\epsilon$.

```ad-note
**Randomness of Confidence Interval**

For each data set, we see one random sample slope, $\hat{\beta_{j}}$ , and a random standard error estimator $\text{se}(\hat{\beta}_{j})$. If I had collected another data set with the same true slope $\beta$, I would have had a different vector $\hat{\beta}$ and a different standard error $\text{se}(\hat{\beta}_{j})$, because I would have had **different respondents** in my sample.

Hence, our data set is *one possible world*. We usually only collect one data set, but the data set could have been different, and that’s the point! Across 95% of data sets, a 95% confidence interval will capture the true value of $\beta_{j}$ 95% of the time.

![[Pasted image 20240912002208.png|500]]
```

#### Connection between Confidence Interval and Hypothesis Test 
If our alternative is **two-sided** and we conduct a test with significance level $\alpha$, we can **reject** the null $\gamma$ iff $\gamma$ **DOES NOT fall** within the $100 (1 − \alpha)\%$ confidence interval.
- Correspondingly, $\gamma$ **is IN our confidence interval** iff we failed to reject the null that $\beta_{j}=\gamma_0$ with a two sided alternative 

Formally, we fail to reject iff $$\begin{align}|t_{\text{stat}}| &\le t_{1-\frac{\alpha}{2},n-p-1} \\\hat{\beta_j}-t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j}) &\le \gamma_0 \le \hat{\beta_j}+t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j})\end{align}$$ Note that this is also the definition of a $100 (1-\alpha)\%$ confidence interval.

In a one-sided test (consider $H_{1}:\beta_{j} > 0$), we only reject when the $t$ -statistic is **sufficiently large**. In particular, we reject when $\mathbb{P}(T_{n-p-1} \ge t_{\text{stat}}) \le \alpha \iff t_{\text{stat}} > t_{1-\alpha, n-p-1}$ .
- In other words, we fail to reject when $t_{\text{stat}} \le t_{1-\alpha, n-p-1}$

