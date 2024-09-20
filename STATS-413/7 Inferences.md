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

Then, $$\begin{align} \text{Var}(\hat{\beta}_{\text{Advert, South}}) &= \text{Var}(a^{T} \hat{\beta}) \\ &= \sigma_{\epsilon}^{2} a^{T} (X^{T}X)^{-1} a \\ &= \text{Var}(\hat{\beta})_{(5,5)} + \text{Var}(\hat{\beta})_{(8,8)} + 2\text{Var}(\hat{\beta})_{(5,8)}
\\ &= \text{Var}(\hat{\beta}_{\text{Advert}}) + \text{Var}(\hat{\beta}_{\text{South}}) + \text{Cov}(\hat{\beta}_{\text{Advert}}, \hat{\beta}_{\text{South}})
\\ &\implies \text{stdev}(\hat{\beta}_{\text{Advert, South}}) = \sigma_{\epsilon} \sqrt{a^{T} (X^{T}X)^{-1} a} \\ &\implies \text{se}(\hat{\beta}_{\text{Advert, South}}) = \hat{\sigma}_{\epsilon} \sqrt{a^{T} (X^{T}X)^{-1} a} \end{align}$$

We **CAN** calculate this statistic if we have the MSE $\hat{\sigma}_{\epsilon}^2$ and the design matrix $X$.
```

#### Recap: Inference for Expectations
Suppose you are interested in the mean of a population and you have collected $y_{1},\dots, y_{n}$ which are **iid** and **normally distributed** with $\mathbb{E}(y)=\mu_y$ and $\text{Var}(y) = \sigma^{2}$.

Then, we estimate $\mu_y$ by $\hat{\mu}_{y} = \overline{y} = \frac{1}{n}\sum_{i=1}^{n}y_i$.
- $\text{Var}(\hat{\mu}_{y})= \frac{\sigma^{2}}{n}$
- $\text{stdev}(\hat{\mu}_{y})= \frac{\sigma}{\sqrt{n}}$
- $\text{se}(\hat{\mu}_{y}) = \frac{\hat{\sigma}}{\sqrt{n}}$
where $$\hat{\sigma} = \sqrt{\frac{1}{n-1} \sum_{i=1}^{n} (y_{i}-\bar{y})^{2}}$$
We form confidence intervals and perform hypothesis tests for $\mu_{y}$ using $\hat{\mu}_{y}$, $\text{se}(\hat{\mu}_y)$ and $t_{n-1}$ distribution. 

#### Inference for Conditional Expectations
