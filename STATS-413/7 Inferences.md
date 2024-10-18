[[2024-09-17]] #Regression 

### Relationship between $t_{\text{df}}$ and $\mathcal{F}_{1,\text{df}}$ Distributions
Suppose $T \sim t_{\text{df}}$. Then
$$T^{2} \sim \mathcal{F}_{1, \text{df}}$$

In words, the square of a $t_\text{df}$ -distributed random variable follows an $\mathcal{F_{1, \text{df}}}$ distribution. 

```ad-example
**Example**: $\mathcal{F}$-test and $t$-test

![[Pasted image 20240917233350.png|400]]

The $\mathcal{F}$-statistic is the square of the $t$-statistic, and both produces the same $p$-value as observed [[6 Categorical Variables, Interactions#^dc651b|here]].
```

---
### Linear Combinations of Slope Coefficients

^99db9f

It is true that $$a^{T}\hat{\beta} = \sum\limits_{j=0}^{p} a_{j+1} \hat{\beta_j}$$ where $a=[a_{1},\dots, a_{p}]^T$.

We've been studying a special case to date:
- Setting $a_{j+1}=1$ and the remaining entries zero returns just $\hat{\beta_j}$

The more general form will be useful for, among other things:
1. Inference for **slope coefficients** for the **non-reference category** when doing regression with interactions
2. Inference for $\mathbb{E}(Y|X= \tilde{x})$

```ad-example
**Example**: Slopes with Interactions

![[Pasted image 20240917234654.png|400]]

What if I want to do inference for the slope of advertisement in the southern region? $$\hat{\beta}_{\text{Advert, South}}=\hat{\beta}_{\text{Advert}}+\hat{\beta}_{\text{South}}$$

This does not correspond to one row in the table (`R` output), so we’ll need to do some extra work.

Note that $$\text{Var}(\hat{\beta}_{\text{Advert, South}})=\text{Var}(\hat{\beta}_{\text{Advert}})+\text{Var}(\hat{\beta}_{\text{South}})+2\text{Cov}(\hat{\beta}_{\text{Advert}}, \hat{\beta}_{\text{South}})$$

While we have the variances of both terms ready, we need to compute their covariances.

Now, let's define $a$ as: $$a_{i} = \begin{cases} 1 & i = 5, 8 \\ 0 & \text{otherwise} \end{cases}$$ which gives that $\hat{\beta}_{\text{Advert, South}} = a^T \hat{\beta}$.

Then, $$\begin{align} \text{Var}(\hat{\beta}_{\text{Advert, South}}) &= \text{Var}(a^{T} \hat{\beta}) \\ &= \sigma_{\varepsilon}^{2} a^{T} (X^{T}X)^{-1} a \\ &= \text{Var}(\hat{\beta})_{(5,5)} + \text{Var}(\hat{\beta})_{(8,8)} + 2\text{Var}(\hat{\beta})_{(5,8)}
\\ &= \text{Var}(\hat{\beta}_{\text{Advert}}) + \text{Var}(\hat{\beta}_{\text{South}}) + \text{Cov}(\hat{\beta}_{\text{Advert}}, \hat{\beta}_{\text{South}})
\\ &\implies \text{stdev}(\hat{\beta}_{\text{Advert, South}}) = \sigma_{\varepsilon} \sqrt{a^{T} (X^{T}X)^{-1} a} \\ &\implies \text{se}(\hat{\beta}_{\text{Advert, South}}) = \hat{\sigma}_{\varepsilon} \sqrt{a^{T} (X^{T}X)^{-1} a} \end{align}$$

We **CAN** calculate this statistic if we have the MSE $\hat{\sigma}_{\varepsilon}^2$ and the design matrix $X$.

In `R` code, this is

![[Pasted image 20240919234153.png|400]]

In short, if we need to make inferences on some linear combinaion of coefficients $i, j$, the method above comes in handy in allowing us to compute the necessary statistics, $\hat{\beta}_{i+j}=a^{T}\hat{\beta}$ and $\text{se}(a^{T}\hat{\beta})$.

To avoid running the complicated procedures above, we could set the reference category to one of the two coefficients we're interested when running the regression, under which scenario we will only need to deal with one coefficient, whose statistics are included as parts of the regression output.
```

#### Recap: Inference for Expectations
Suppose you are interested in the mean of a population and you have collected $y_{1},\dots, y_{n}$ which are **iid** and **normally distributed** with $\mathbb{E}(y)=\mu_y$ and $\text{Var}(y) = \sigma^{2}$.

Then, we estimate $\mu_y$ by $\hat{\mu}_{y} = \overline{y} = \frac{1}{n}\sum_{i=1}^{n}y_i$. ^5965d1
- $\text{Var}(\hat{\mu}_{y})= \frac{\sigma^{2}}{n}$
- $\text{stdev}(\hat{\mu}_{y})= \frac{\sigma}{\sqrt{n}}$
- $\text{se}(\hat{\mu}_{y}) = \frac{\hat{\sigma}}{\sqrt{n}}$ where the [[4 Hypothesis Tests#^52cf89|root mean squared error]] is $$\hat{\sigma} = \sqrt{\frac{1}{n-1} \sum_{i=1}^{n} (y_{i}-\bar{y})^{2}}$$ ^8e3c23

We form confidence intervals and perform hypothesis tests for $\mu_{y}$ using $\hat{\mu}_{y}$, $\text{se}(\hat{\mu}_y)$ and $t_{n-1}$ distribution.  ^a53087

#### Inference for Conditional Expectations
Now, suppose $y_1,\dots,y_n$ are generated from the (**stronger**) linear model: ![[4 Hypothesis Tests#^bfc3d4]]
For any particular value for the predictors $\tilde{x}=[1, \tilde{x}_{1}, \dots ,\tilde{x}_{p}]^{T}$: $$\mu_{y|\tilde{x}}=\mathbb{E}(y|x=\tilde{x})=\tilde{x}^{T}\beta$$
After running OLS regression and obtaining my estimate $\hat{\beta}$: $$\hat{\mu}_{y|\tilde{x}}=\hat{\mathbb{E}}(y|x=\tilde{x})=\tilde{x}^{T}\hat{\beta}$$
Before digging deeper, we need to understand the distinction between inferences on the **population mean** $\mu_{y}$ and **conditional expectation** $\mu_{y|x}$.
- Inference on $\mu_{y}$ is an **unconditional expectation**
- Inference on $\mu_{y|\tilde{x}}$ is a **conditional expectation**

It's obvious that $$\mathbb{E}(\hat{\mu}_{y|\tilde{x}})=\mu_{y|\tilde{x}}$$ and $$\text{Var}(\hat{\mu}_{y|\tilde{x}}) = \sigma_{\varepsilon}^{2} \tilde{x}^{T}(X^{T}X)^{-1}\tilde{x}$$
Recall that if $Z$ follows a multivariate normal distribution, so too does $AZ + c$. So under the stronger linear model, we also have $$\hat{\mu}_{y|\tilde{x}} \sim N(\mu_{y|\tilde{x}}, \sigma_{\varepsilon}^{2} \tilde{x}^{T}(X^{T}X)^{-1}\tilde{x})$$
We can further estimate $\text{stdev}({\hat{\mu}_{y|\tilde{x}}})$ using the following standard error: $$
\text{se}(\hat{\mu}_{y|\tilde{x}})=\hat{\sigma_{\varepsilon}}\sqrt{\tilde{x}^{T}(X^TX)^{-1}\tilde{x}}$$
Note that the standard error is a function of $\tilde{x}$. Hence, it is minimized when there is the **smallest variability**: $\tilde{x} = [1, \bar{x}_{1}, \dots, \bar{x}_{p}]$ - the mean of the predictor variables. Variability increases as $\tilde{x}$ moves farther from the mean.

We may construct a $100 (1-\alpha)\%$ $t$ -based confidence interval for $\mu_{y|\tilde{x}}$:  $$\hat{\mu}_{y|\tilde{x}} \pm t_{1-\alpha/2,n-p-1}\text{se}(\hat{\mu}_{y|\tilde{x}})$$
The intervals are centered at the **prediction** from our estimated regression line, $\tilde{x}^T \hat{\beta}$. ^cbe93c

Likewise, under the stronger linear model, we could form a test statistic to test the null $H_{0}: \mu_{y|\tilde{x}} = \mu_{0}$ of the form $$t_{\text{stat}}= \frac{\hat{\mu}_{y|\tilde{x}}-\mu_0}{\text{se}(\hat{\mu}_{y|\tilde{x}})}$$ and compute $p$ -values using a $t_{n-p-1}$ distribution. 

The intervals tend to be more popular in practice than the tests.

```ad-example
**Example**: Confidence Intervals for Conditional Expectations

![[Pasted image 20240919230758.png|400]]

Here, `fit` is the prediction from the regression line. `lwr` and `upr` are the loer and upper bous of the 95% confidence interval.

![[Pasted image 20240919230929.png|400]]

This agrees with our previous intuition - we have more information about what’s occurring near the **center** of my data (lower SE and thus smaller confidence interval).
- Intervals centered at $\hat{\mu}_{y|\tilde{x}}=\tilde{x}^{T}\hat{\beta}$
- Narrowest at mean of covariates
- Wider at the extremes
```
