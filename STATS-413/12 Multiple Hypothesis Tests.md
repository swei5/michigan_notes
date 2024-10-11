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
do while 1:
	if p(i) <= a/(K-i+1):
		i += 1
		continue
	else:
		return
```

If the method terminates in step $1$, we don’t reject any null hypotheses. If it terminates in step $i$ , you reject the hypotheses corresponding to $p_{1},\cdots, p_{n}$. If it doesn’t terminate, we reject all K of our hypotheses.