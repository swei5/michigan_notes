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
We’ve previously discussed algebraic properties of $e$ and $\hat{y}$ when running a regression with an intercept. Let $\text{Cov}(\cdot, \cdot)$  be the **sample** covariance operator. Then, $$\text{Cov}(e,x_{j})=0$$ $$\text{Cov}(e,\hat{y})=0$$ (shown [here](https://stats.stackexchange.com/q/474587)). And $$\bar{e}=\frac{1}{n}\sum\limits_{i=1}^{n}e_{i}=0$$ (shown using the [[2 Multiple Regression#^81c207|Normal Equation]])

These properties are purely algebraic, and hold **regardless** of whether the weaker linear model holds.

We’ll now derive additional properties of $e$ and $\hat{y}$ under the assumption that the weaker or stronger linear model holds. These properties represent **testable implications** of these models.

```ad-important
**Definition 9.1**: Expectation of Residuals and Fitted Values

Under the weaker linear model $$\mathbb{E}(e)=\mathbb{E}((I-H)y)=\mathbf{0}$$ and $$\mathbb{E}(\hat{y})=\mathbb{E}(Hy)=X\beta$$
```

Note the distinction between $\mathbb{E}(e_{i})=0$ and $\overline{e}=0$;  the former is a statement about the **expectation for ANY particular residual**; The latter merely says that the **sample average of the residuals** equals zero in any particular data set.

```ad-important
**Definition 9.2**: Variances of Residuals and Fitted Values

Under the weaker linear model $$\text{Var}(e)=\sigma_{\epsilon}^{2}(I-H)$$ and $$\text{Var}(\hat{y})=\sigma_{\epsilon}^{2}H$$
```

Keep in mind the distinction between $\text{Var}(e_{i})=\sigma_{\epsilon}^{2}(1-h_{ii})$ and $\hat{\sigma}_{\epsilon}^{2}= \frac{1}{n-p-1} \sum_{i=1}^{n} e^{2}$. The former describes **variability for ANY component** $e_{i}$ across data sets. The latter is the **sample variance** for the observed residuals.

#### Hat Matrix
When performing regression with an intercept and $p$ predictors, the **diagonal entries** of the hat matrix $H$, $h_{ii}$ , satisfy the following $$\frac{1}{n} \le h_{ii} \le 1$$ for $i=1,\dots,n$. And $$\sum\limits_{i=1}^{n}h_{ii}=p+1$$
This implies that $$\text{Var}(e_{i})=\sigma_{\epsilon}^{2}(1-h_{ii}) \le \sigma_{\epsilon}^{2}=\text{Var}(\epsilon_{i})$$
In addition, for any $i, j=1,\dots,n$ $$\text{Cov}(e_{i},e_{j})=-\sigma_{\epsilon}^{2}h_{ij}$$ $$\text{Cov}(\hat{y}_{i},\hat{y}_{j})=\sigma_{\epsilon}^{2}h_{ij}$$
