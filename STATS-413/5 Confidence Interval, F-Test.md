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

The corresponding `R` command for $t_{1-\frac{\alpha}{2},n-p-1}$ is `qt(1-a/2, n-p-1)`. 

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

Formally, we fail to reject iff $$\begin{align}|t_{\text{stat}}| &\le t_{1-\frac{\alpha}{2},n-p-1} \\\hat{\beta_j}-t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j}) &\le \gamma_0 \le \hat{\beta_j}+t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j})\end{align}$$ Note that this is also the definition of a $100 (1-\alpha)\%$ confidence interval: $$[\hat{\beta_j}-t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j}),\hat{\beta_j}+t_{1-\frac{\alpha}{2},n-p-1}\text{se}(\hat{\beta}_{j})]$$

```ad-note
To reject $H_{0}\ne \gamma_{0}$, we need $\mathbb{P}(T_{n-p-1} \le |t_{\text{stat}}|) \le \alpha$, which is equivalent to saying that $\hat{\beta}_{j}$ is outside of the $100(1-\alpha)\%$ confidence interval of $\gamma_0$ - and if we construct the same $100(1-\alpha)\%$ confidence interval this time for $\hat{\beta}_{j}$, naturally, $\gamma_0$ would be outside the interval using the same $t$ distribution given the same parameters $\alpha$ and $\text{df}=n-p-1$.
```

In a one-sided test (consider $H_{1}:\beta_{j} > 0$), we only reject when the $t$ -statistic is **sufficiently large**. In particular, we reject when $\mathbb{P}(T_{n-p-1} \ge t_{\text{stat}}) \le \alpha \iff t_{\text{stat}} > t_{1-\alpha, n-p-1}$ .
- In other words, we fail to reject when $t_{\text{stat}} \le t_{1-\alpha, n-p-1}$

Formally, we fail to reject iff $$\begin{align}t_{\text{stat}} &\le t_{1-\alpha,n-p-1} \\ \gamma_0 &\ge \hat{\beta_j}-t_{1-\alpha,n-p-1}\text{se}(\hat{\beta}_{j})\end{align}$$
This means that our $100 (1-\alpha)\%$ one-sided "greater than" confidence interval has the form $$[\hat{\beta_j}-t_{1-\alpha,n-p-1}\text{se}(\hat{\beta}_{j}),\infty)$$

---
### $\mathcal{F}$ -test
We now know how to, given a set of explanatory variables $x_1,\dots,x_p$, create a multiple regression model to try to predict the value of the response $y$. We want to know if there is evidence that the model *helped*.
- Relative to having **NO information** on the explanatory variables - simply predicting the overall mean

As of now, he test will be based on the magnitude of [[3 Projections, OLS Estimators, Variability#$R {2}$|R-squared]]: *how much variation in $y$ could we explain from our regression*?

Under the assumptions of the multiple regression model, the null hypothesis for testing “was multiple regression worth it?” takes on the form: $$H_{0}: \beta_1=\dots=\beta_{p}=0$$
And the alternative is $$H_{a} = \text{ at least one } \beta_{j}\ne0$$
Another way to think about this: under the null, the only thing that matters for predicting a response is the **intercept**, $\beta_0$.
- If this were true, $\mathbb{E}(y_{i}|x_{i}) = \beta_{0}$; that is, every point has the **same mean** regardless of the values of the explanatory variables

We’ll form a test statistic whose value is large when $x_1,\dots,x_p$ are strong predictors of $y$ (large values mean strong evidence against the null). This corresponds to large values for R-squared.

```ad-important
**Definition 5.2**: $\mathcal{F}$-statistic

The statistic takes on the form: $$F_{\text{stat}}=\frac{R^{2}}{1-R^{2}}\left(\frac{n-p-1}{p}\right)$$
```

$R^2$ will be greater than zero due to chance correlations, even if the explanatory variables don’t actually matter - and, the more predictors we add to the model, the higher the $R^2$ will be.

#### $\mathcal{F}$ -Distribution
It turns out that, **under the null hypothesis**, the distribution of $F_{\text{stat}}$ is a member of a family of probability distributions known as $\mathcal{F}$ -distributions.
- Parametrized by **degrees of freedom** (two parameters of $\text{df}$)
- **NOT symmetric**

Formally, $F_{\text{stat}}$ follows a $\mathcal{F}_{p, n-p-1}$ distribution, where $p$ is the number of predictor variables and $n$ is the sample size.

We calculate the $p$ -value as $\mathbb{P}(\mathcal{F}_{p, n-p-1} \ge F_{\text{stat}})$: here the extremes are always in the **right tail**.

```ad-example
**Example, F-Test**

![[Pasted image 20240913002519.png|400]]

Hence, this suggests that **our regression model provides a statistically significant** improvement in predictions for sales.
```

The overall $\mathcal{F}$ -test can be viewed as a comparison between:
1. The residual sum of squares in a model including **ALL of the predictor variables**, and
2. The residual sum of squares in a model only including those variables whose coefficients **don’t equal zero under the null** ($\beta_0$)

In other words, the objective of the test is to quantify **how large of a discrepancy** between [[3 Projections, OLS Estimators, Variability#^cb791d|TSS]] and [[3 Projections, OLS Estimators, Variability#^dbce55|RSS]] we should expect under the null. If what we observe is **much larger** than expected, we reject the null. Alternatively, we may write
$$F_{\text{stat}}=\frac{\frac{TSS-RSS}{p}}{\frac{RSS}{n-p-1}}=\frac{\frac{TSS-RSS}{p}}{\hat{\sigma}_{\epsilon}^2}$$
Numerator represents the **difference between the TSS and the RSS**, divided by the differences in the degrees of freedom for the residual terms in those two regressions:
1. Regression with **ONLY an intercept**: $e$ has $n-1$ degrees of freedom
2. Regression with **ALL variables**: $e$ has $n-p-1$ degrees of freedom

The difference between (1) and (2) is $p$.

Denominator represents **RSS in the full model**, divided by degrees of freedom for residual vector in the full model. 
- Note that this is the **mean squared error** of the model, previously introduced [[4 Hypothesis Testing#^52cf89|here]]

---
### Covered `R` Functions

```r
# Compute F-stat
1-pf(Fstat, df1=p, df2=n-p-1)

# p-val computation
1-pt(tstat, n-p-1) # Greater than
pt(tstat, n-p-1) # Less than
2pt(−|tstat|, n-p-1) # Two-sided

# Two-sided CI
# beta can be accessed through coeff
# se can be accessed through standard.errors
beta+c(-1,1)*qt(1-a/2,n-p-1)*se 

# Or,
confint(lm, level = .95)
```
