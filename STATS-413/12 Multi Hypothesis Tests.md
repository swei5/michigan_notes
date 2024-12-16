[[2024-10-03]] #Regression 

### Multiple Hypothesis Tests
By now, under the **[[4 Hypothesis Tests#^bfc3d4|stronger linear model]]** we know how to
1. Conduct [[4 Hypothesis Tests|hypothesis tests]] for slope coefficients
2. Construct [[7 Inferences|confidence intervals for conditional expectations]]
3. Construct [[8 Prediction Intervals|prediction intervals]] for future observations

These procedures depend upon a parameter, $\alpha$, which is meant to control the [[4 Hypothesis Tests#^1ba045|Type I error]] rate. All is fine when we conduct a **single** hypothesis test / construct a **single** confidence interval. In our tests for outliers, we mentioned how one needs to [[10 Outliers#^9bf0ba|account for the number of hypotheses]] being tested to avoid committing too many Type I errors.

Proper calculation of $p$ -values can be troublesome when we want to simultaneously answer multiple research questions.
- The more you look for something unusual to occur, the more likely you will find it (Data Mining)

```ad-example
**Example**: xkcd Experiment

Suppose I have data on 1000 study participants. 500 of them are randomly assigned to eat no jelly beans for two months. The remaining 500 are split into 20 groups with 25 participants each. In each group, patients are assigned a particular color of jelly bean to consume. They eat 5 jelly beans a day of their assigned color over the course of two months. All 1000 participants count the total number of pimples they’ve had over the course of the study period.

Here, we have 20 hypotheses (for each color) to test for: 
- $H_{0j}:\beta_{j}=0$
- $H_{aj}:\beta_{j}\ne0$

However, if we are testing $K$ null hypotheses that are actually true, and the hypothesis tests are **independent** from one another, the probability that we commit **AT LEAST one** type one error is $1-(1-\alpha)^{K}$.
```

When testing multiple hypotheses, we clearly need to make an adjustment to control this rate of false positives.

```ad-important
**Definition 12.1**: Familywise Error Rate

The **Familywise Error Rate** is the probability of rejecting at least one null hypothesis that is actually true when testing $K$ hypotheses. That is, it’s the **probability of committing at least one** Type I error.
```

Often, we want to control the familywise error rate at $\alpha=0.05$.

There’s a simple way to correct for multiple comparisons and guarantee that the familywise error rate is **below a pre-specified threshold**d regardless of the dependence between tests (sometimes test statistics are correlated): ![[10 Outliers#^9bf0ba]]
```ad-summary
**Differences between $\mathcal{F}$-test and Bonferroni**

The overall $\mathcal{F}$-test would provide us with a test of the null that, if we have $G$ categories for a categorical variable, $$\begin{align}\\
H_{0}&:\mu_{1}=\mu_{2}=\cdots =\mu_{G}\\
H_{a}&: \text{ at least one } \mu_{i} \ne \mu_{j}
\end{align}$$

If we reject this null, we certainly might be interested in the **precise nature** of the differences, i.e. **which groups** are different?

Explicitly, for any $i , j$, we might want to assess the null: $$H_{0}:\mu_{i}=\mu_{j}$$

Through using the Bonferroni correction, we are also entitled to talk about the **individual results** of all of comparisons we have made.

If we rejected a null hypothesis even after applying a Bonferroni correction, we’d be allowed to say that the difference was statistically significant while **accounting for the potential impact of multiple comparison**.

In contrast to the overall $\mathcal{F}$-test, which tells us something is going on but doesn’t let me talk about the particular nature of the differences.
```

When using a Bonferroni correction when testing $K$ hypotheses, we reject the null when $p<\alpha/K$. Unfortunately, more stringent criterion also makes it **less likely that we’ll correctly reject** an incorrect null hypothesis.
- That is, the **power** of the resulting hypothesis test has been **reduced**

#### Holm-Bonferroni
Despite its simplicity, it turns out there’s a method that is uniformly better than Bonferroni known as Holm-Bonferroni, which controls the familywise error rate, while potentially providing improvements in power.

```ad-important
**Definition 12.2**: Holm-Bonferroni

We first order the $p$-values from $K$ hypothesis tests as $p_{1} \le p_{2}\le \cdots \le p_{K}$. Then,

```r
i = 1
do while:
	if p(i) <= a/(K-i+1):
		i += 1
		continue
	else:
		exit
```

If the method terminates in step $1$, we don’t reject any null hypotheses. If it terminates in step $i$ , you reject the hypotheses corresponding to $p_{1},\cdots, p_{i-1}$. If it doesn’t terminate, we reject all $K$ of our hypotheses.
- This method rejects **as many or more** hypotheses than Bonferroni, which compares all $p$ -values to $\alpha/K$

---
### Pairwise Comparisons
Multiple regression with a single categorical predictor and no continuous predictors (like our jelly bean example) is sometimes referred to as **One-Way Analysis of Variance**, or One-Way ANOVA.
- Large assumption when using ANOVA: **equal variances across the groups** being compared

In this context, there are particular methods built for comparing the average responses across different groups while controlling the familywise error rate.

Suppose we have $G$ groups ($G$ levels of the categorical predictor). For all $i\ne j, i, j=1,\cdots,G$ , we might want to assess the null: $$H_{0}:\mu_{i} =\mu_{j}$$ We want to make all $K=\binom G2$  possible pairwise comparisons between different groups while controlling the familywise error rate at level $\alpha$.

```ad-important
**Definition 12.3**: Tukey-Kramer Honest Significant Difference

Let $\bar{y}_{i}$ be the observed mean in the $i$th of $G$ groups, and let $\hat{\sigma}_{\varepsilon}$ be the RMSE from our regression of the response on the categorical predictor.

For all $i,j$, we can construct a $100(1-\alpha)\%$ confidence interval for $\mu_{i}-\mu_{j}$ while controlling the familywise error rate at $\alpha$ through the following: $$\bar{y}_{i}-\bar{y}_{j} \pm \frac{q_{1-\alpha,G,n-G}} {\sqrt{2}} \hat{\sigma}_{\varepsilon} \sqrt{\frac{1}{n_{i}} + \frac{1}{n_{j}}} $$ where $q_{1-\alpha,G,n-G}$ is a quantile from the **Studentized Range Distribution**, and $n_{i}$ is the sample size for the $i$th group and $\hat{\sigma}_{\varepsilon} \sqrt{\frac{1}{n_{i}} + \frac{1}{n_{j}}}$ is the standard error of $\text{se}(\bar{y}_{i}-\bar{y}_{j})$.
```

Studentized Range $(G , n-G)$ accounts for the fact that we are looking at $|\bar{y}_{i}-\bar{y}_{j}|$ for **all possible combinations of groups**.

We use it to produce **wider intervals** than those from the $t$ distribution unless $G=2$.

```ad-example
**Comparing Tukey to Bonferroni**

For a given $G$ , consider $95\%$ confidence intervals in One-Way ANOVA.
- Tukey-Kramer: $q_{0.95,G,n-G}/\sqrt{2}$
- Bonferroni: $t_{1-0.025/K,n-G}$ where $K= \binom G2$

Let us fix $n=1000$ for purpose of illustration:

![[Pasted image 20241010203652.png|500]]

Quantiles are the same for $G=2$; Tukey-Kramer is better (narrower CI) for $G>2$.
```

---
### Scheffé's Method
In regression, we’ve introduced methods for constructing 95% confidence intervals for $\mu_{y|\tilde{x}}=\mathbb{E}(y|\tilde{x})$ at particular data point $\tilde{x}$. There are called **pointwise confidence intervals**. This says that for any particular point $\tilde{x}$, we are 95% confident that $\mu_{y|\tilde{x}}$ will fall within $$\hat{\mu}_{y|\tilde{x}} \pm t_{1-\alpha/2,n-p-1}\text{se}(\hat{\mu}_{y|\tilde{x}})$$
However, this does not imply that we are 95% confident that **the true prediction equation** falls within these bounds. This would be making multiple comparisons!

In other words, rather than capturing $\mu_{y|\tilde{x}}$ at a given point $\tilde{x}$, can we create upper and lower bounds that capture the entire function $\mathbb{E}(y|\tilde{x})=\tilde{x}^{T}\beta$ with $100 (1-\alpha)\%$ confidence?

```ad-important
**Definition 12.4**: Scheffé's Method

At any point $\tilde{x}$, consider the following modified confidence intervals for the conditional expectation: $$\hat{\mu}_{y|\tilde{x}} \pm \sqrt{(p+1)\mathcal{F}_{1-\alpha, (p+1), n-p-1}} \hat{\sigma}_{\varepsilon} \sqrt{\tilde{x}^{T}(X^{T}X)^{-1}\tilde{x}}$$ where $\mathcal{F}_{1-\alpha, (p+1), n-p-1}$ is the $1-\alpha$ quantile of an $\mathcal{F}$ distribution with $p+1$ and $n-p-1$ degrees of freedom.
```

Scheffé's Method replaces a quantile from the $t_{n-p-1}$ distribution with $\sqrt{(p+1)\mathcal{F}_{1-\alpha, (p+1), n-p-1}}$ .
- This accounts for looking at **all possible linear combinations** of $\hat{\beta}$ and $\tilde{x}^T \hat{\beta}$
- Size of penalty **depends on the number of predictor variables**
	- Large $p$ results in very **wide** intervals - much wider than pointwise intervals

The resulting intervals, when plotted for all values of $\tilde{x}$, capture the **true prediction equation** with $100 (1-\alpha)\%$ confidence.

![[Pasted image 20241017212738.png|500]]
