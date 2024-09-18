[[2024-09-12]] #Regression #R 

### Categorical Variables

```ad-important
**Definition 6.1**: Categorical Variables

Categorical (also called nominal or qualitative) variables are ones in which the ‘values’ = are **labels for group memberships**. These values are often (but not always) coded as words.

Categorical variables that can only take on **two** distinct values are called **binary variables**.
```

If we have a categorical variable that takes on $G$ values, `R` creates $G − 1$ binary variables (also called dummy variables) and includes them as regressors.

```ad-example
**Regression with Categorical Variables, Example**

![[Pasted image 20240914192056.png|500]]
```

The group that `R` doesn’t include an indicator for is known as the **reference category**.
- The interpretation of the slope coefficients is made relative to (in reference to) the **excluded category**

Suppose my categorical predictor variable takes on $G$ values, $c_{1}, \dots, c_{G}$ and my equation is of the form $$\hat{y}_{i}=\hat{\beta}_{0}+\hat{\beta}_{1} \mathbb{1}\{x_{i} =c_{1}\}+\dots+\hat{\beta}_{G-1} \mathbb{1}\{x_{i} =c_{G-1}\}$$ Such that $c_{G}$ is the reference category. The **expected difference** in values for $y$ for two individuals, one in category $c_{j}$ and one in $c_{G}$ is $\hat{\beta}_{j}$.
- E.g. prediction for group $c_{1}$: $\hat{\beta_{0}}+\hat{\beta}_{G-1}$ and prediction for reference group is simply $\hat{\beta}_{0}$

We can't include $G$ dummy variables. Recall the normal equation ![[2 Multiple Regression#^81c207]]
And our vector of slope coefficients ![[2 Multiple Regression#^b72bff]]
We need $(X^{T}X)^{-1}$ to be invertible, which requires
- $p+1 <n$, and
- Linear independence between the columns of $X$

If $G$ variables are included - then $d_{1}+d_{2}+ \dots + d_{G}=\mathbf{1}_{n \times G}$ (assume each observation only falls into $1$ of the $G$ categories)
- Note that the first column of the design matrix $X$, the intercept column, is already $\mathbf{1}_{n \times G}$ - violates linear independence

The **population** expectation under the multiple regression model would be $$\mathbb{E}(y_i|X_{i})=\beta_{0}+\beta_{1} \mathbb{1}(x_{i}=c_{i}) + \dots + \beta_{G-1}\mathbb{1}(x_i=c_{G-1})$$ And under the null hypothesis tested by the overall $\mathcal{F}$, $$H_{0}:\beta_{1}=\dots = \beta_{G-1}$$ The expectation function would instead be $$\mathbb{E}(y_{i}|x_{i})=\beta_0$$ i.e. the expectation is the **same for all categories**
- So, the overall $\mathcal{F}$ -test from this regression can be used to assess the null $H_{0}:\mu_{g}=\beta_{0}$ for all $g$, where $\mu_{g}$ is the population mean in the $g$ th category; in other words, we are asking if the **means from all categories are the SAME**

Recall the definition of $\mathcal{F}$ -test: ![[5 Confidence Interval, F-Test#^5e5647]]

We calculate TSS based on the **overall mean** for all categories ($\bar{y}$), and RSS based on the **mean of each category** ($\hat{y}$).
#### Analysis of Variance
This regression-based approach for testing the equality of means through use of the overall $\mathcal{F}$ -test has its own name in the literature: **Analysis of Variance**, or ANOVA for short.

Note that the test is assuming the truth of the *stronger* regression model. Most importantly,
- Homoskedasticity: $\text{Var}(y_{i})=\sigma_{\epsilon}^{2}$
- Normality: $\epsilon_{i} \sim N(0,\sigma_{\epsilon}^{2})$

In this context, homoskedasticity means $\sigma_{1} = \dots = \sigma_{G}$.

---
### Interactions
It may be that the **slope of a predictor variable changes as a function of another categorical variable**.
- The continuous variable may impact the expectation in different groups differently

```ad-example
**Example**: Regression with both Categorical and Continuous Variables

Assume this is our regrssion equation.

![[Pasted image 20240916211846.png|500]]

In the **south**, for two territories with the **same advertising spend** but who differ in total bonus paid by 1 hundred dollars is expected to **differ in** $1.534$ in sales.

In the **midwest**, for two territories with the **same advertising** spend but who differ in total bonus paid by 1 hundred dollars is expected to **differ in** $1.534$ in sales, **ALSO**.

This implies that **across different groups**, regression lines have different intercepts, but **SAME slopes**! This is **NOT always the case in reality**.
```

We can capture these differences through the incorporation of **interaction terms**.

```ad-important
**Definition 6.2**: Interaction Terms

To form an interaction term, we simply take the **product** of the two covariates whose interaction we are investigating: $X_{i} \times X_{j}$.

The interaction term between a continuous and categorical covariate takes on the following form: $$X_{i} \mathbb{1}\{X_{j}=c\}=\begin{cases} 0 & X_{j}\ne c \\ X_{i} & X_{j}=c
\end{cases}$$

Each interaction term is a $n \times 1$ column vector.
```

```ad-example
**Example**: Regressions with Interactions

![[Pasted image 20240916213539.png|500]]

`:` in the covariate term means *to interact with*. At this extreme, each group has its **own slope and intercepts**.
- Midwest (reference): $\hat{\beta}_{0}+\hat{\beta}_{\text{bonus}}(\text{Bonus}) + \hat{\beta}_{\text{advert}}(\text{Advert})$
- South: $(\hat{\beta}_{0}+\hat{\beta}_\text{south}) + (\hat{\beta}_{\text{bonus}} + \hat{\beta}_{\text{bonus:SOUTH}})(\text{Bonus}) + (\hat{\beta}_{\text{advert}} + \hat{\beta}_{\text{advert:SOUTH}})(\text{Advert})$

This is **NO DIFFERENT** to saying if we fit three separate regressions each with each group, except for potential differences in standard errors and corresponding $t$-values.
```

Through this process we’ve built up from a fairly simple model for sales to one that separately models sales according to region. Nevertheless, we want to ask - *did we need* to do this to the full extent we’ve provided here?

---
### Partial $F$ -test
The required test is called a partial $\mathcal{F}$ -test. 
- Partial in the sense that it **compares large (full) models to some partial subset** of the large model

Suppose we want to fit a model $y_{i}=\beta_{0}+\beta_{1}x_{i1}+ \dots + \beta_{p}x_{ip} +\epsilon_{i}$ and re interested in the null hypothesis that some subset of slopes $\mathcal{I} \subseteq \set{1,\dots, p}$ are zero: $$H_{0}:\beta_{i}=0 \text{ for some subset of covariates } \mathcal{I}$$The alternative hypothesis is then $$ H_{a}: \text{at least one slope in subset } \mathcal{I} \text{is non zero}$$ **AT LEAST** $\beta_{i}$ **AND/OR** $\beta_j$ is nonzero.

As contrasted to **ALL slopes** as demonstrated in a *full* $\mathcal{F}$ -test ![[5 Confidence Interval, F-Test#^589891]]
In other words, a partial $\mathcal{F}$ -test is a natural extension of the $t$ -tests on **individual coefficients** to dealing with **sets of coefficients**.

```ad-note
If the subset is a **single** coefficient, a partial $\mathcal{F}$-test and a $t$-test will be **identical**, giving the same $p$-score.
```

^dc651b

A natural way to test this is to compare the RSS from a regression **including ALL variables** to the RSS from a regression **excluding the variables** whose slopes are zero under the null:
1. **Full Model**: The model including the full set of covariates
	- $RSS_{\text{full}}$: the RSS in the full model, including all candidate predictor variables
	- $\text{df}_\text{full}$: the degrees of freedom for the residual vector under the full model: $n-p_\text{full}-1$
1. **Reduced Model**: The model only including the covariates whose slopes aren’t set to zero under the null
	- $RSS_\text{red}$: the RSS in the reduced model, **including only the predictor variables whose slope coefficients aren’t equal to zero** under the null
	- $\text{df}_\text{red}$: $n-p_\text{red}-1$

Note that $p_\text{full} > p_\text{red}$.

```ad-example
If we wanted to see if we needed interactions between region and bonus, we’d compare a model including them (the full model) to a model excluding them (the reduced model)
```

```ad-important
**Definition 6.3**: Partial $\mathcal{F}$-test

The test statistic may be expressed as: $$F_\text{stat}=\frac{(RSS_\text{red}-RSS_\text{full})/(\text{df}_\text{red}-\text{df}_\text{full})}{RSS_\text{full}/\text{df}_{\text{full}}}$$

Under the null: $F_{\text{stat}} \sim \mathcal{F}_{(\text{df}_{\text{red}} - \text{df}_\text{full}), \text{df}_\text{full}}$.

View the definition of a [[5 Confidence Interval, F-Test#^5e5647|full F-test here]] and see the similarity.
```

The numerator is the **difference between the RSS** in the two models, divided by the differences in the degrees of freedom for the residual terms in those two regressions. Again, the denominator is the **MSE** in the **full model**.

```ad-note
We may view the overall $\mathcal{F}$-test as a partial $\mathcal{F}$-test where the reduced model only contains the intercept, i.e. $p=0$ for this rergression. Then,
- $\text{df}_\text{red} = n-1$
- $RSS_\text{red} = TSS$

And hence $$\begin{align}F_\text{stat}=\frac{(RSS_\text{red}-RSS_\text{full})/(\text{df}_\text{red}-\text{df}_\text{full})}{RSS_\text{full}/\text{df}_{\text{full}}}&=\frac{(TSS-RSS_\text{full})/p}{RSS_\text{full}/(n-p-1)}\\&=\left(\frac{R^2}{1-R^2}\right)\left(\frac{n-p-1}{p}\right)\end{align}$$
```

The overall $\mathcal{F}$ -test acts as a proof showing that there's **AT LEAST SOMETHING** going on that makes doing regression useful in the first place - to verify that this model isn't *overcomplicated* - we conduct partial $\mathcal{F}$ -test.

```ad-example
**Example**: Conducting Partial $\mathcal{F}$-test

To conduct the test, we fit the full and reduced linear models, and feed the results into the `anova` command.

![[Pasted image 20240917142549.png|500]]

Conclusion: **Fail to reject** the null. It suggests that we don’t need to include interactions between region and bonus in the model.
```

---
### Continuous Interactions
Interaction effects are **NOT** solely limited to interactions between continuous variables and categorical variables. 

The interaction term between two continuous variables takes on the form: $$X_{i}:X_{j}=X_{i}\times X_{j}$$
```ad-example
**Example**: Regression with Continuous Interactions

Assume the output looks like this: ![[Pasted image 20240917144715.png|500]]

Then, our prediction equation given the interaction term will be $$\text{sales}=205.0266+5.6312(\text{income})-61.6539(\text{competitors})+0.5841(\text{income}\cdot \text{competitors})$$

Since we only include one interaction term, the results from the $t$-test: ![[Pasted image 20240917145007.png|500]]

Is exactly the result from partial $\mathcal{F}$-test where the reduced model where the interaction term is not introduced.

![[Pasted image 20240917145605.png|400]]
```

---
### Covered `R` Functions

```r
# Regression with interactions
lm(Sales~Region*Bonus + Region*Advert))

# Partial F-test
lm.full = lm(Sales~Bonus*Region + Advert*Region)
lm.reduced = lm(Sales~Bonus + Advert*Region)

anova(lm.reduced, lm.full)
```
