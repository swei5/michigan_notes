[[2024-09-05]] #Regression 

### Hypothesis Testing, Recap
When conducting statistical inference, we are informally trying to answer the following question:
- Is what I’m observing real, or could it be attributable to random chance alone?

Our pursuit begins with both a question which we collect data to try to answer:
1. The **Alternative Hypothesis** is what we, the researcher, want to show
2. The **Null Hypothesis** is the opposite of what we want to show. Oftentimes, this represents the status quo. The purpose of research is often to further knowledge by refuting this commonly held belief

In order to prove a new hypothesis, one does not actually collect evidence to confirm it; rather, we proceed backwards: data is collected to **falsify the opposite** of what we want to prove. We end up using a device known as a **proof by contradiction** (falsification, refutation).
- We may **NEVER *accept*** the null hypothesis, rather, we either **refute** it or **failed to refute** it

#### Tests of Significance
This involves a statistical hypothesis test: if the null hypothesis were true, what’s the chance of observing evidence **as extreme or more extreme** than what we observed in our data if randomness alone were the explanation?

```ad-important
**Definition 4.1**: $p$-value

A $p$-value is defined to be the probability of observing a result that is **as extreme or more extreme** than what we have actually observed in our sample or experiment, **given that the null hypothesis is actually true**.
```

The $p$ -value is **NOT** the probability that null hypothesis is true. The truth of the null hypothesis is **constant** but **unknown**.

Once we compute the appropriate p-value, we need to determine if it is **substantial** enough to reject the null hypothesis.

```ad-important
**Definition 4.2**: Significance Level

The significance level of a hypothesis test, denoted as $\alpha$, is a significance threshold specified before the analysis begins such that if the $p$-value falls at or below $\alpha$, we **reject** the null hypothesis.

If $p$-value is greater than $\alpha$, then we **fail to reject** the null hypothesis since we lack substantial evidence.

Convention is to use $\alpha = 0.05$.
```

#### Types of Errors
We can characterize the types of correct or incorrect decisions in a 2 × 2 table: 
![[Pasted image 20240905122427.png|400]]

A **Type I Error** occurs if we **reject the null hypothesis** when it was actually true.
- E.g. declaring the defendant guilty when they are innocent

A **Type II Error** occurs if we **fail to reject the null hypothesis** when it was actually false.
- E.g. declaring the defendant not guilty, when they are actually guilty

Note that the significance level directly determines the Type I error rate.
- We reject $H_{0}$ when $p\le \alpha$, hence $\mathbb{P}(\text{Type 1}|H_{0}) \le \alpha$

Type II errors are that the null was incorrect is actually true, but they weren’t able to show it based on their test. For a **given significance level**, increasing sample size $n$ increases the **power** of a well-designed test, where
$$\text{power}=1-P(\text{Type II Error})$$
Generally, we try to devise tests that have the largest power possible, subject to **controlling the Type I Error** rate at $\alpha$.

In one-sample hypothesis testing, we use the $z$ -statistic when the **population standard deviation** is **known**, and use the $t$ -statistic when the **population standard deviation** is **unknown.**
$$p=\mathbb{P}(T \ge \text{t-stats} | H_{0})$$
---
### Hypothesis Testing for Slope
Consider a linear model $y = X \beta + \epsilon$ with $p$ predictors, and suppose we wanted to test whether or not the slope on covariate $x_j$ , $\beta_j$ , was equal to a particular value:
$$H_{0}:\beta_{j}=\gamma_{0}$$
The alternatives take the following form:
- $H_{a}: \beta_{j}<\gamma_{0}$: less than
- $H_{a}: \beta_{j}>\gamma_{0}$: greater than
- $H_{a}: \beta_{j} \ne \gamma_{0}$: **two-sided**

By far the most common hypothesis test for slope coefficients is $\gamma_{0}=0$.

As previously introduced [[2 Multiple Regression#Interpretation of Intercepts||here]], $\beta_j$ requires *ceteris paribus* - having the same values for all other predictor variables fixed. As such, hypothesis test for the null $\beta_{j}=\gamma_{0}$ can be thought of as:
- After **controlling / adjusting for the effect** of all other predictor variables $\set{x_{l}; l \ne j}$, let’s assess evidence for whether or not the true slope on the $j$ th variable is $\gamma_{0}$

#### Stronger Linear Model
While the assumptions that $E (\epsilon) = 0$ and $\text{Var}(\epsilon)=\sigma_{\epsilon}^{2}$ are enough for unbiasedness and to derive a form for the variance for $\hat{\beta}$, we’ll impose **additional restrictions** on $\epsilon$ to derive a sampling distribution for $\hat{\beta}$:
$$\epsilon \sim MVN(0,\sigma_{\epsilon}^{2}I)$$
Where $MVN$ represents the multivariate normal distribution. This is equivalent to saying that
$$\epsilon_{i} \sim N(0,\sigma_{\epsilon}^{2})$$
And that $\epsilon_{i}$ are iid and normally distributed with expectation 0 and variance $\sigma_{\epsilon}^{2}$.

```ad-example
**Example**: Stronger Linear Model ($p=1$)

![[Pasted image 20240906001708.png|300]]
```

```ad-note
In summary, the stronger linear model assumes:
1. **Linearity**: $\mathbb{E}(y_{i})=x_{i}^{T}\beta$
2. **Homoskedasticity**: $\text{Var}(y_{i})=\sigma_{\epsilon}^{2}$
3. **Normality**: $\epsilon_{i} \sim N(0,\sigma_{\epsilon}^{2})$
```

#### Distribution of $\hat{\beta}$
Under the stronger linear model,
$$\hat{\beta} \sim MVN(\beta, \sigma_{\epsilon}^{2}(X^TX)^{-1})$$
And similarly
$$\hat{\beta_{j}} \sim N(\beta_{j},\sigma_{\epsilon}^{2}(X^TX)^{-1}_{(j+1),(j+1)})$$
The mean and standard deviation of $\hat{\beta}$ are previously derived here ![[3 Projections, OLS Estimators, Variability#^a4deed]]
And here:
![[3 Projections, OLS Estimators, Variability#^89d509]]

So, for any slope coefficient $j$, letting $\text{stdev}(\hat{\beta}_{j})=\sigma_{\epsilon} \sqrt{(X^TX)^{-1}_{(j+1),(j+1)}}$ and
$$\frac{\hat{\beta}_{j}-\beta_{j}}{\text{stdev}(\hat{\beta}_{j})} \sim N(0,1)$$
If we knew $\sigma_{\epsilon}^{2}$, we could begin with the value of the OLS slope coefficient returned by `R`: $\hat{\beta}_{j}^{\text{obs}}$ and compute $z$ -statistic using d
$$z_{\text{stat}}=\frac{\hat{\beta}_{j}^{\text{obs}}-\gamma_{0}}{\text{stdev}(\hat{\beta}_{j})}$$
And finally compute $p$ -values using tail probabilities from $N$.

Unfortunately, we don’t know $\sigma_{\epsilon}$ - it is a **parameter** of the linear model and hence we can't compute $z_{\text{stat}}$ since we can't compute $\text{stdev}(\hat{\beta}_{j})$. We would need to estimate $\sigma_{\epsilon}$ in the first place.

```ad-important
**Definition 4.3**: Root Mean Squared Error (RMSE)
$$\hat{\sigma_{\epsilon}}=\sqrt{\frac{\sum_{i=1}^{n}e_{i}^{2}}{n-p-1}}$$

where $n-p-1$ are referred to as the **degrees of freedom** for the residual vector $e$.
- $e$ is constrained by $p+1$ equations $e^{T}X=0$
- Residual vector is constrained to an $n − p − 1$ dimensional subspace
```

With an estimation for $\sigma_{\epsilon}$ , we can now estimate $\text{stdev}(\hat{\beta}_{j})$ using $\text{se}(\hat{\beta}_{j})$, the **standard error** for the $j$ th coefficient:$$\text{se}(\hat{\beta}_{j})=\hat{\sigma_{\epsilon}}\sqrt{(X^TX)^{-1}_{(j+1),(j+1)}}$$ ^bde6fa
#### $t$ -Statistic
With this, we’ll consider a new test statistic which replaces $\text{stdev}(\hat{\beta}_{j})$ with its sample analogue $\text{se}(\hat{\beta}_{j})$$$t_{\text{stat}}=\frac{\hat{\beta}_{j}^{\text{obs}}-\gamma_{0}}{\text{se}(\hat{\beta}_{j})}$$ ^cecd4f
```ad-important
**Definition 4.4**: A Null Distribution for Testing $\beta_{j}$

Under the null hypothesis and assuming the **stronger linear model**, tstat follows a $t$ distribution with $n − p − 1$ degrees of freedom.
```

We then use tail probabilities from the $t_{n−p−1}$ distribution to compute $p$ -values. 

The $t$ family of distributions is indexed by a number known as its “degrees of freedom,” or $df$ for short. As $df \to \infty$, the $t_{df}$ converges to a **standard normal distribution**.

![[Pasted image 20240906114003.png|400]]

---
### Covered `R` Functions

```r
# Calculate t-test TAIL probability
pt(tstat, n − p − 1)

# Retrieve standard errors
summary(lm.medicorp)$coefficients[,2]
```

#### `lm` Output Interpretation

![[Pasted image 20240906114330.png|500]]

- $\hat{\beta}$: first column
- [[#^bde6fa|Standard Errors]]: second column
- [[#^cecd4f|t-statistic]]: third column
	- Note that this is **BASED ON** $\gamma_{0}=0$
- One-tail probability: fourth column