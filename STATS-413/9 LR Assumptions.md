[[2024-09-24]] #Regression 

To date we have derived properties of the ordinary least squares coefficients $\hat{\beta}$ under the assumption that a **linear model holds**.

```ad-summary
The weaker linear model states $$\begin{align}y&=X\beta+\epsilon \\ \mathbb{E}(\epsilon)&=0 \\ \text{Var}(\epsilon) &= \sigma_{\epsilon}^{2}I \end{align}$$

If this holds, we showed among many things:
- $\mathbb{E}(\hat{\beta})=\beta$
- $\text{Var}(\hat{\beta})=\sigma_{\epsilon}^{2}(X^{T}X)^{-1}$
- $\mathbb{E}(y-X\hat{\beta})=0$

If we further assume (stronger linear model) that $\epsilon \sim MVN(0, \sigma_{\epsilon}^{2}I)$, we can construct
- [[5 Confidence Intervals|Hypothesis tests/confidence intervals for slope coefficients]]
- [[6 Categorical Variables|Hypothesis tests to compare models of differing complexity]]
- [[7 Inferences|Confidence intervals for the conditional expectation]]
- [[8 Prediction Intervals|Prediction intervals for future observations]]
```

Violations of these assumptions can invalidate the methods we’ve proposed. For instance: 
- $\mathbb{E}(y-X\hat{\beta})\ne0$
	- Our hypothesis tests might commit a [[4 Hypothesis Tests#^1ba045|Type I error]] at a rate different from $\alpha$
	- The coverage of our confidence intervals might differ from $100 (1-\alpha)\%$
	- ...

### Regression Diagnostics
Regression Diagnostics are tools/methods for determining whether a regression model adequately represents the data at hand. It takes two main forms:
1. **Diagnostic Plots** - create a plot which can visually reveal a violation of a modeling assumption
2. **Diagnostic Tests** - test the null hypothesis that a given modeling assumption holds; Rejection of the null suggests a violation of the assumption under consideration

We will focus on graphical diagnostics here.

It is difficult to visualize the relationship between $y$ and $x$ unless $p=1$ - we need to consider low-dimensional consequences of the linear model.

#### Algebraic Properties of Residuals and Fitted Values
Recall that $H=X(X^{T}X)^{-1}X^{T}$ and let $h_{ij}$ denote its $i,j$ element. Then,  $$e=(I-H)y$$ And $$\hat{y}=Hy$$
We’ve previously discussed algebraic properties of $e$ and $\hat{y}$ when running a regression with an intercept. Let $\text{Cov}(\cdot, \cdot)$  be the **sample** covariance operator. Then, $$\text{Cov}(e,x_{j})=0$$ $$\text{Cov}(e,\hat{y})=0$$ (shown [here](https://stats.stackexchange.com/q/474587)). And $$\bar{e}=\frac{1}{n}\sum\limits_{i=1}^{n}e_{i}=0$$ (shown using the [[2 Multiple Regression#^81c207|Normal Equation]]) ^c048e6

These properties are purely algebraic, and hold **regardless** of whether the weaker linear model holds.

#### Properties under Linear Model Assumptions
We’ll now derive additional properties of $e$ and $\hat{y}$ **under the assumption** that the weaker or stronger linear model holds. These properties represent **testable implications** of these models.

```ad-important
**Definition 9.1**: Expectation of Residuals and Fitted Values

Under the weaker linear model $$\mathbb{E}(e)=\mathbb{E}((I-H)y)=\mathbf{0}$$ using $\mathbb{E}(y-Hy)=\mathbb{E}(y-X\hat{\beta})=0$ and $\mathbb{E}(\hat{\beta})=\beta$, $\mathbb{E}(y-X\beta)=0$ under assumptions; and $$\mathbb{E}(\hat{y})=\mathbb{E}(Hy)=X\beta$$ using $Hy=X\hat{\beta}$ and $\mathbb{E}(\hat{\beta})=\beta$ under assumptions.
```

Note the distinction between $\mathbb{E}(e_{i})=0$ and $\overline{e}=0$;  the former is a statement about the **expectation for ANY particular residual**; The latter merely says that the **sample average of the residuals** equals zero in any particular data set.

```ad-important
**Definition 9.2**: Variances of Residuals and Fitted Values

Under the weaker linear model $$\text{Var}(e)=\sigma_{\epsilon}^{2}(I-H)$$ using $\text{Var}[(I-H)y]=(I-H)\text{Var}(y)(I-H)^{T}$ and $$\text{Var}(\hat{y})=\sigma_{\epsilon}^{2}H$$ using $\text{Var}(\hat{y})=\text{Var}(Hy)=H\text{Var}(y)H^{T}$ and $\text{Var}(y)=$.
```

Keep in mind the distinction between $\text{Var}(e_{i})=\sigma_{\epsilon}^{2}(1-h_{ii})$ and $\hat{\sigma}_{\epsilon}^{2}= \frac{1}{n-p-1} \sum_{i=1}^{n} e^{2}$. The former describes **variability for ANY component** $e_{i}$ across data sets. The latter is the **sample variance** for the observed residuals.

When performing regression with an intercept and $p$ predictors, the **diagonal entries** of the hat matrix $H$, $h_{ii}$ , satisfy the following $$\frac{1}{n} \le h_{ii} \le 1$$ for $i=1,\dots,n$. And $$\sum\limits_{i=1}^{n}h_{ii}=p+1$$
This implies that $$\text{Var}(e_{i})=\sigma_{\epsilon}^{2}(1-h_{ii}) \le \sigma_{\epsilon}^{2}=\text{Var}(\epsilon_{i})$$ In words, residuals **DO NOT** generally have the **same variance** as errors do; in addition, residuals $e_{i}$ have **SMALLER** variance then $\epsilon_{i}$.

In addition, for any $i, j=1,\dots,n$ $$\text{Cov}(e_{i},e_{j})=-\sigma_{\epsilon}^{2}h_{ij}$$ $$\text{Cov}(\hat{y}_{i},\hat{y}_{j})=\sigma_{\epsilon}^{2}h_{ij}$$
This implies that despite the fact $\text{Cov}(\epsilon_{i},\epsilon_{j})=0$ under the weaker linear model, $\text{Cov}(e_{i}, e_{j})\ne 0$.

```ad-important
**Definition 9.3**: Properties of Covariance

For $A \in \mathbb{R}^{d\times m}, B \in \mathbb{R}^{k \times n}$: $$\text{Cov}(AZ, BY)=A \text{Cov}(Z,Y)B^{T}$$ for random vectors $Z \in \mathbb{R}^{m}, Y \in \mathbb{R}^{n}$.

Note that [[3 OLS Estimators, Variability#^2cf564|Definition 3.5]] is a special case of this.
```

```ad-important
**Definition 9.4**: Covariance of Residuals and Fitted Values

Using this definition, we can characterize the covariance between residuals and fitted values under the weaker linear model: $$\begin{align}\text{Cov}(e,\hat{y})=&\text{Cov}((I-H)y, Hy)\\&=(I-H)\text{Cov}(y,y)H^{T} \\&= \text{Cov}(y,y)(I-H)H^{T} \\ &= \text{Cov}(y,y) 0 & (H^{T}=H, HH=H) \\&= \textbf{0} \end{align}$$

In other words, for $i, j =1, \dots, n$, $\text{Cov}(e_{i},\hat{y}_{j})=0$.
```

This is **distinct** from the result that the sample covariance between $e$ and $\hat{y}$ equals zero [[#^c048e6|when running regression with an intercept]].
- $\text{Cov}(e_{i},\hat{y}_{j})=0$: Property of random variables across data sets **under the linear model**
- $\text{Cov}(e, \hat{y})=0$ : Algebraic property

