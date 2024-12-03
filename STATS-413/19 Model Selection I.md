[[2024-11-07]] #Regression #R

Through our discussion of nonlinear regression (Lec 15-19), we understand the importance of choosing the right **level of complexity**.
- [[16 Overfitting|Bias-Variance Trade-Off]]

In linear regression, complexity rises with the **number of predictor variables** that we choose to include in our model.
- Want to include any predictor variable that’s truly meaningful for predicting the response (otherwise there would be **bias**)
- Want to ignore irrelevant predictors (these would just add noise to our predictions, increasing **variance**)

Imagine that we have $p$ covariates, and that linearity holds:  $$\mathbb{E}(y|x)=x^T \beta$$ Of the covariates $x_{1},\cdots, x_{p}$, we imagine that only some of them have nonzero slope coefficients.
- If $\beta_{j}\ne 0$, we’d like to use that variable for prediction

Our objective is to find the subset $\mathcal{M}_{m}\subseteq \{1,\cdots, p\}$ of $m \le p$ indices for covariates with nonzero slope coefficients.

There are $2^{p}$ possible models, each corresponds to a distinct subset $\mathcal{M}$, as for each variable, we may include or not include. In other words, for a given model size $0 \le m \le p$, we could imagine looking at all $\binom{p}{m}$ models with those $m$ predictor variables. For each possible subset, we
1. Run a regression of $y$ on those $m$ predictor variables
2. Calculate the $RSS$ for that model (or $R^{2}$)

Of all models of size $m$, we could then choose the model that minimized the $RSS$, or equivalently maximized the $R^{2}$. Let's call this value $RSS^{\star}_{m}$
- Nevertheless, note that a key deficiency of $R^{2}$ is that it is not able to compare models of different complexities, with differing numbers of covariates

If we did the previous for each model size $m$, we would have a collection of **best residuals sums of squares** $RSS_{0}^{\star},\cdots, RSS_{p}^{\star}$.
- We know that $RSS_{0}^{\star}\ge RSS_{1}^{\star} \ge \cdots \ge RSS_{p}^\star$ as in-sample fit will improve as the number of predictors increases

We’ll now describe some common approaches to variable selection:
1. [[#Hypothesis Test-Based Methods|Hypothesis Test-Based Methods]]
2. [[#Criterion-Based Methods|Criterion-Based Methods]]

---
### Hypothesis Test-Based Methods
Recall that when running linear regression, recall that a test of the null that $\beta_{j}=0$ had the following interpretation:
- After controlling for the other predictor variables besides $x_{j}$, is there evidence that $x_{j}$ substantially improves the predictive power of our model
	- In other words, do we need to include $x_{j}$ once we’ve already included the other $p-1$ variables
	- Under the stronger linear model, we can conduct this hypothesis test using a $t$ -statistic

This is also a particular instance of a partial $\mathcal{F}$ -test: ![[6 Categorical Variables#^4b24d8]]
In practice, we often conduct these tests by **stepwise regression** using $p$ -values.

```ad-important
**Definition 19.1**: Backwards Elimination

Backwards elimination starts with a linear model including all predictors, and then tries to **reduce** the model’s size.
1. Start by fitting a regression using all $p$ predictor variables
2. Check to see if any $p$-values **fall above** your significance level $\alpha$
3. If some variables have $p$-values larger than $\alpha$, remove the single predictor variable with the largest $p$-value
4. Re-fit your regression using the remaining $p-1$ predictors and return to step 2
5. Stop when you have a model where all variables have $p$-values below $\alpha$

In `R`, the function `ols_step_backward_p` in the package `olsrr` executes this routine.
```

^bdb940

Alternatively, we can also adapt **forward selection**.

```ad-important
**Definition 19.2**: Forward Selection

Forward Selection starts with a model only including the intercept, and then iteratively **adds variables** to create a larger model.
1. Start by fitting a regression using only the intercept
2. For all predictors not currently in the model, check the $p$-value for those predictors if they are added to the model
3. If any of these $p$-values are below $\alpha$, add the one with the smallest $p$-value below
4. Refit the model with the new variable added, return to step 2
5. Stop when no new variables would yield p-values below

In `R`, the function `ols_step_forward_p` in the package `olsrr` executes this routine.
```

^7398ef

When we have categorical predictors, we generally consider adding **either all** of the categories or **none** of the categories to the model.
- Remember: we encode categoricals with $d-1$ dummy variables for $d$ categories
- We should group these $d-1$ variables as a whole - when deciding whether to add categoricals, should look at the $p$ -value for the partial $\mathcal{F}$ -test where the **categories are included in full model**, excluded in the reduced model

```ad-note
Backwards elimination and forward selection aren’t guaranteed to produce the same model - in terms of model size and predictors selected.
- This is because of multicollinearity, and its impact on the interpretation of slope coefficients
```

A few drawbacks of this approach are
- Approach isn’t guaranteed to provide the model with the best **out-of-sample performance**
	- Models tend to be too small
	- Impacted by multicollinearity
- We cannot take the $p$ -values in the chosen model at face-value (as we have conducted multiple comparisons cheated our way through)

---
### Criterion-Based Methods
Alternative approaches to model selection imagine optimizing an objective function which encourages model fit while penalizing model complexity. Consider $$\arg \min\limits_{\text{all models}} \text{Criterion}$$ where the criterion:
- Is decreasing as **quality of the fit** to the observed data increases
- Is increasing as the **complexity** of the model increases (measured by $m$)

#### Adjusted $R^{2}$

```ad-important
**Definition 19.3**: Adjusted $R^{2}$

For a given model $\mathcal{M}_{m}$ with $m$ predictors and residual sums of squares $RSS$, the adjusted $R^{2}$ is defined as $$R^{2}_{adj}(\mathcal{M}_{m})=1-\frac{RSS(\mathcal{M}_{m})/(n-m-1)}{TSS/(n-1)}=1-\frac{\hat{\sigma}^{2}_{\varepsilon}(\mathcal{M}_{m})}{\text{stdev}(y)^{2}}$$

Note its difference from [[3 OLS Estimators#^723b57|raw R-squared]].

This benefits as a measure of improvement as
- $R^{2}_{adj}$ can decrease as we add more variables - this is because of the inclusion of the degrees of freedom, which penalizes models with larger $m$, making it more suitable for model selection
- $R^{2}_adj$ can be **negative**!
```

We can rewrite the above as $$\hat{\sigma}^{2}_{\varepsilon}(\mathcal{M}_{m})=\frac{RSS(\mathcal{M}_{m})}{n-m-1}=\text{stdev}(y)\sqrt{1-R^{2}_{adj}}$$ Hence, maximizing $R^{2}_{adj}$ is equivalent to **minimizing** $\hat{\sigma}^{2}_{\varepsilon}(\mathcal{M}_{m})$.

Consider then choosing a model through $$\arg \min\limits_{\text{all models}} \frac{RSS(\mathcal{M}_{m})}{n-m-1}$$
This approach promotes **model fit** (numerator) and penalizes model complexity (denominator).

#### Mallows’s $C_{p}$

```ad-important
**Definition 19.4**: Mallows's $C_p$

For a model $\mathcal{M}_{m}$ with $m$ predictors, Mallows’s $C_{p}$ takes the form $$C_{p}(\mathcal{M}_{m})=\frac{RSS(\mathcal{M}_{m})}{\hat{\sigma}^{2}_\varepsilon(\mathcal{M}_{p})}+2(m+1)-n$$ where $\hat{\sigma}^{2}_\varepsilon(\mathcal{M}_{p})$ is estimated from the model with all $p$ predictor variables.
```

^a6739d

By intuition, $C_{p}$ provides an estimate of the (scaled) residual sums of squares: $$\frac{1}{\sigma_\varepsilon ^{2}} \sum\limits_{i=1}^{n}(\hat{y}_{i}-x_{i}^{T}\beta)^{2}$$ where $\hat{y}_{i}$ are the fitted values from model $\mathcal{M}_{m}$.
- In a good fit, $C_{p}$ should be near or below $m+1$

#### Information-Based Criteria
Under the **stronger linear model**, theoretical derivations can be used to derive additional criterion which take complexity into account. For a model $\mathcal{M}_{m}$ with $m$ predictors:

```ad-important
**Definition 19.5**: Akaike Information Criterion (AIC)

$$AIC(\mathcal{M}_{m})=n \ln \left(\frac{RSS(\mathcal{M}_{m})}{n}\right)+2m$$
```

```ad-important
**Definition 19.6**: Bayesian Information Criterion (BIC)

$$BIC(\mathcal{M}_{m})=n \ln \left(\frac{RSS(\mathcal{M}_{m})}{n}\right)+\ln(n)m$$
```

Note that $\ln (8)\approx 2.079$ - for $n \ge 8$, BIC penalizes larger models with more predictor variables more than AIC.

For a fixed model size $m$, all of the aforementioned criterion would agree on the same *best model* - model of size $m$ that yields the **SMALLEST** $RSS$.
- Their difference lies in the nature of the **penalty for complexity** (on $m$)

---
### Best Subset Selection
For a chosen criterion, if we could evaluate all possible models:
1. For a given model size $m$, evaluate all $\binom{p}{m}$ models and find he set of m predictors yielding the best values for the criterion; call the best value $\text{Criterion}^{\star}_{m}$
	- All four criteria we’ve introduced would agree upon the **same best model** of size $m$
1. Across values for $m$, find the minimal value for $\text{Criterion}^{\star}_{m}$
	- This is where the differences between criteria would show up - different penalizations on $m$ yield **different models**

This general idea is called **Best Subset Selection**, and is implemented in the `regsubsets` command in the `leaps` package with options for all four of the criteria we’ve mentioned.

```ad-note
In general, adjusted $R^{2}$ chooses **larger** models, BIC chooses smaller models, and AIC/Mallows’ $C_{p}$ falls in between.
```

The above may work out and produce an optimal solution for a relatively small number of covariates. However, it becomes **computationally challenging** for $p$ moderate or large.
- Suggests finding greedy heuristics for the above schema

---
### Stepwise Regression

^4ee7e8

An instinct may be to employ an **iterative model** building schema to explore the set of all possible models, just like we did with the [[#Hypothesis Test-Based Methods|hypothesis test based approaches]]. This is common in practice, known as **stepwise regression**, with three contenders:
1. Forward Selection ^cb2fc5
2. Backwards Elimination ^f29005
3. Hybrid Elimination (can be helpful under multicollinearity)

```ad-important
**Definition 19.7**: Forward Selection, Complexity-adjusted

Let $\mathcal{M}_{0}^{\star}$ be the initial model with **NO covariates** (only includes the intercept). For $m=1,\cdots,p$:
1. For each of the variables not included in $\mathcal{M}^{\star}_{m-1}$, consider a new model of size $m$ which adds that particular variable to $\mathcal{M}^{\star}_{m-1}$
2. Among these **candidate new models**, let $\mathcal{M}^{\star}_{m}$ be the model with $m$ predictors resulting in the **best value for the complexity-adjusted criterion**
3. Calculate the complexity-adjusted criterion for $\mathcal{M}^{\star}_{m-1}$ and $\mathcal{M}^{\star}_{m}$
4. If the criterion improved
	- Store $\mathcal{M}^{\star}_{m}$ as the new model, continue
	- Else, stop and return the model $\mathcal{M}^{\star}_{m-1}$
```

```ad-important
**Definition 19.8**: Backwards Elimination, Complexity-adjusted

Let $\mathcal{M}_{p}^{\star}$ be the initial model with **ALL $p$ covariates** included. For $m=p-1,\cdots,0$:
1. For each of the variables not included in $\mathcal{M}^{\star}_{m+1}$, consider a new model of size $m$ which adds that particular variable to $\mathcal{M}^{\star}_{m+1}$ (keeping all other variables in $\mathcal{M}^{\star}_{m+1}$)
2. Among these **candidate new models**, let $\mathcal{M}^{\star}_{m}$ be the model with $m$ predictors resulting in the **best value for the complexity-adjusted criterion**
3. Calculate the complexity-adjusted criterion for $\mathcal{M}^{\star}_{m+1}$ and $\mathcal{M}^{\star}_{m}$
4. If the criterion improved
	- Store $\mathcal{M}^{\star}_{m}$ as the new model, continue
	- Else, stop and return the model $\mathcal{M}^{\star}_{m+1}$
```

```ad-important
**Definition 19.9**: Hybrid Elimination

Hybrid elimination combines backwards and forwards steps. At each step:
1. Consider **all possible models attained by adding** a predictor to the existing model
2. Consider **all possible models attained by removing** a given predictor from the existing model
3. Make whatever change results in the smallest value for the complexity-adjusted criterion
```

The function `step` implements forward stepwise, backwards elimination, and hybrid elimination for AIC and BIC.
- Can use this function to also select based upon Adjusted $R^2$ and $C_{p}$ by varying the parameter `k`

```ad-note
All criteria **agree on** which predictor should be added (forward selection) or removed (backward elimination).

The only difference is the **condition under which algorithm terminates**.
```

```r
# forward, AIC
step(lm.empty, scope = list(lower = lm.empty, upper = lm.full), direction = "forward")

# backward, BIC
step(lm.full, scope = list(lower = lm.empty, upper = lm.full), direction = "backward", k = log(n))
```
