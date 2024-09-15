[[2024-09-12]] #Regression 

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