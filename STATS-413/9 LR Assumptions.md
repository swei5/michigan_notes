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