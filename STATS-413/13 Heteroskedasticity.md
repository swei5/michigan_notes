[[2024-10-17]] #Regression #R

In the first part of this course, we
- Derived certain consequences of the ordinary least squares slope coefficients under the assumptions of the weaker and stronger linear models (Lecture 1-8)
- Discussed diagnostics to assess whether these assumptions seem reasonable (Lecture 9-11)

Now: What happens if certain **assumptions seem unreasonable**? Can we proceed? If so, how?

### Heteroskedasticity
We’ll begin by assessing the implications of heteroskedasticity for **estimation and inference**.

```ad-summary
The weaker linear model states $$\begin{align}y&=X\beta+\varepsilon \\ \mathbb{E}(\varepsilon)&=0 \\ \text{Var}(\varepsilon) &= \sigma_{\varepsilon}^{2}I \end{align}$$

If this holds, we showed among many things:
- $\mathbb{E}(\hat{\beta})=\beta$
- $\text{Var}(\hat{\beta})=\sigma_{\varepsilon}^{2}(X^{T}X)^{-1}$
- $\mathbb{E}(y-X\hat{\beta})=0$
```

When discussing heteroskedasticity, we will consider the following generative model: $$\begin{align}
y &= X\beta + \varepsilon\\
\mathbb{E}(\varepsilon) &= 0\\
\text{Var}(\varepsilon)&=\sigma_{\varepsilon}^{2}W^{-1} 
\end{align}$$ where $\varepsilon_{1},\cdots, \varepsilon_{n}$ are independent, $W$ is a $n \times n$ diagonal matrix with $w_{ii}>0$ on the diagonals (zeros off the diagonals) ^e387c1
- Implies $W^{-1}$ is a $n \times n$ diagonal matrix with $1/w_{ii}$ on the diagonals (zeros off the diagonals)
- $W=W^{-1}=I$ implies homoskedasticity

Suppose the previous model for heteroskedasticity holds; then: $$\begin{align}
\mathbb{E}(\hat{\beta}) &= \beta\\
\text{Var}(\hat{\beta})&=\sigma_{\varepsilon}^{2}(X^TX)^{-1}X^{T}W^{-1}X(X^TX)^{-1} \\
&\ne \sigma_{\varepsilon}^{2}(X^TX)^{-1}
\end{align}$$

```ad-note
The consequences of the above include that
- $\hat{\beta}$ is still **unbiased** for $\beta$ under heteroskedasticity
- The usual standard errors (including associated $t$-scores and $p$-values) reported by `R` for $\hat{\beta}$ are **INVALID**; they can be too large or too small
	- These standard errors are based upon $\hat{V}(\hat{\beta})=\hat{\sigma}_{\varepsilon}^{2}(X^TX)^{-1}$, which was justified under homoskedasticity

Even if $\varepsilon$ is multivariate normal, the usual hypothesis tests and confidence intervals will also be invalid!
```

We’ll now consider two approaches to dealing with heteroskedasticity, depending upon whether we know the value of $W$:
1. If $W$ is known, we can construct an estimator that actually improves upon the ordinary least squares solution, through a technique called **weighted least squares**
2. If $W$ is **unknown**, we can construct standard errors for the OLS solution $\hat{\beta}$ that are valid even under heteroskedasticity; these are called **heteroskedasticity-consistent** (HC) **standard errors**

---
### Weighted Least Squares
Let's assume that $W$ is known. Let $W^{1/2}$ be a diagonal matrix with $w_{ii}^{1/2}=\sqrt{w_{ii}}$ on the diagonals. Consider the transformation: $$W^{1/2}y=W^{1/2}(X\beta+\varepsilon)$$ In which, 
- $(W^{1/2}y)_{i}=\sqrt{w_{ii}}y_i$
- $(W^{1/2}X)_{ij}=\sqrt{w_{ii}}x_{ij}$

```ad-important
**Definition 13.1**: Homoskedastic After Transformation

$$\begin{align}
\text{Var}(W^{1/2}y)&=\text{Var}(W^{1/2}X\beta+W^{1/2}\varepsilon)\\
&= \text{Var}(W^{1/2}\varepsilon) \\
&= W^{1/2}\text{Var}(\varepsilon)W^{1/2}&W\text{ is symmetric}\\
&= W^{1/2}\sigma_{\varepsilon}^{2}W^{-1}W^{1/2}\\
&= \sigma_{\varepsilon}^{2}I
\end{align}$$
```

Now, consider running an ordinary least squares regression of the **transformed responses** $W^{1/2}y$, on the **transformed predictors** $W^{1/2}X$: 

```ad-important
**Definition 13.2**: WLS Estimator

$$\begin{align}
\hat{\beta}_{WLS}&=\text{arg} \min\limits_{\beta}(W^{1/2}y-W^{1/2}X\beta)^{T}(W^{1/2}y-W^{1/2}X\beta)\\
&= \text{arg} \min\limits_{\beta}(y-X\beta)^T W(y-X\beta)\\
&= \text{arg} \min\limits_{\beta} \sum\limits_{i=1}^{n} w_{ii}(y_{i}-x_{i}^{T}\beta)^{2}\\
&= \left((W^{1/2}X)^{T}W^{1/2}X\right)^{-1}(W^{1/2}X)^{T}W^{1/2}y&\text{ Note similarity with Definition 2.4}\\
&= (X^{T}WX)^{-1}X^{T}Wy
\end{align}$$

This is called the **weighted least squares** solution. Larger $w_{ii}$ means **bigger penalty** on $(y_{i}-x_{i}^{T}\beta)^{2}$ and **stronger emphasis** on fit for individual $i$.
```

```ad-important
**Definition 13.3**: Expectation and Variance of $\hat{\beta}_{WLS}$

$$\begin{align}
\mathbb{E}(\hat{\beta}_{WLS}) &= \beta\\
\text{Var}(\hat{\beta}_{WLS})&=\hat{\sigma}_{\varepsilon}^{2}(X^{T}WX)^{-1}
\end{align}
$$
```

Comparing these results with what we obtained for the [[3 OLS Estimators#^a4deed|ordinary least squares]], the estimators have the **SAME expectations**, but **DIFFERENT variances**. 

Under the new generative model for $y$, it turns out that $\hat{\beta}_{WLS}$ is the best linear unbiased estimator (BLUE) of $\beta$.
- **Linear**: estimators $\tilde{\beta}$ can be written in the form $\tilde{\beta}=Cy$ for some matrix $C$
- **Unbiased**: $\mathbb{E}(\hat{\beta}_{WLS}) = \beta$
- **Best**: For any vector of constants $a$, $\text{Var}(a^{T}\tilde{\beta})$ has the **smallest possible variance** out of all unbiased linear estimators

```ad-important
**Definition 13.4**: Gauss-Markov Theorem

Let $\hat{\beta}_{WLS}$ be the weighted least squares solution. Suppose the above generative model and that $X$ is full rank. Let $\tilde{\beta}$ be another linear unbiased estimator of $\beta$. Then, for any constant vector $a \in \mathbb{R}^{p+1}$: $$\text{Var}(a^{T}\hat{\beta}_{WLS}) \le \text{Var}(a^{T}\tilde{\beta})$$

This **justifies** the usage of $\hat{\beta}_{WLS}$ under heteroskedasticity.
```

^8fffb5

```ad-note
**Proof of Definition 13.4**

![[Pasted image 20241018175619.png|400]]

![[Pasted image 20241018175632.png|400]]
```

Note that if $W=W^{-1}=I$, $\hat{\beta}_{WLS}=\hat{\beta}_{OLS}$.
- Under homoskedasticity, $\hat{\beta}_{OLS}$ is the best linear unbiased estimator for $\beta$
- The derivation above provides additional justification for using OLS to estimate $\beta$ under the weaker and stronger linear model
- Any other linear, unbiased estimator would be worse than $\hat{\beta}_{OLS}$

In the general case where $W \ne I$, we see that $\hat{\beta}_{WLS}$ is preferred to $\hat{\beta}_{OLS}$.

```ad-note
Recall that our model for heteroskedasticity states $$\text{Var}(\varepsilon)=\sigma_{\varepsilon}^{2}W^{-1}$$ so that the variances of $\varepsilon_{i}$ are proportional to the **inverse of the weights**.
- Small $\text{Var}(\varepsilon_{i})$: larger weight for that observation when determining $\hat{\beta}_{WLS}$
	- Believe that $y_{i}$ is on average closer to $\mathbb{E}(y_{i})=x_{i}^T \beta$ so it prioritizes the prediction equation fitting that point better
- Larger $\text{Var}(\varepsilon_{i})$: smaller weight for that observation when determining $\hat{\beta}_{WLS}$
	- $y_{i}$ could be quite far from $\mathbb{E}(y_{i})=x_{i}^T \beta$ due to variability, so it should not prioritize the prediction equation fitting that particular point well
```

We have so far assumed that $W$ is known, but have made no assumption that $\sigma_{\varepsilon}^{2}$ is known. 

```ad-important
**Definition 13.5**: Standard Errors for Weighted Least Squares

If $\sigma_{\varepsilon}^{2}$ is unknown, we can estimate it using its mean squared error (MSE): $$\text{MSE}_{WLS}=\hat{\sigma}_{\varepsilon,WLS}^{2}=\frac{(y-X\hat{\beta}_{WLS})^T W(y-X\hat{\beta}_{WLS})}{n-p-1}$$

We can then estimate $\text{Var}(\hat{\beta}_{WLS})$ by $$\hat{V}(\hat{\beta}_{WLS})=\hat{\sigma}_{\varepsilon,WLS}^{2}(X^{T}WX)^{-1}$$ From here, we can derive **standard errors** for $\hat{\beta}_{j, WLS}$ as $$\text{se}(\hat{\beta}_{j,WLS})=\hat{\sigma}_{\varepsilon,WLS}\sqrt{(X^{T}WX)^{-1}}$$
```

When do we know $W$? For instances:
- If each observation $i$ represents the **average response** among a $n_i$ equally weighted responses in a given group, then $w_{ii}=n_{i}$
	- Can occur when each observation $i$ represents the **number of respondents/participants** at a given location
	- Can occur if running experiments of different sizes at each $x_{i}$
- If it’s known that the standard deviation is **proportional to a predictor variable**, then $w_{ii}=1/x_{i}^{2}$
	- In many systems, **larger values for the predictors introduce more variability** in the responses
	- Would require subject matter knowledge

```ad-note
If we simply view $W$ as a weight matrix, without necessarily believing $\text{Var}(\varepsilon)=\sigma_{\varepsilon}^{2}W^{-1}$, weighted least squares can account for **different priorities** on performance of predictions at different covariates $x_{i}$ $$\hat{\beta}_{WLS}= \text{arg} \min\limits_{\beta} \sum\limits_{i=1}^{n} w_{ii}(y_{i}-x_{i}^{T}\beta)^{2}$$

If we care more about **accuracy** of predictions at certain values of $x_{i}$, we can place a larger weight $w_{ii}$ on the residuals for those observations.
- There could be predictor variables that encode sensitive attributes, or reflect observations of high value to stakeholders

Under this use case for weighted least squares, the Gauss-Markov justification for WLS goes out the window. We are instead arguing that the **loss function underpinning WLS better aligns** with our priorities.
```

---
### HC Standard Errors
If we don't know $W$ and hence $\text{Var}(\varepsilon)$ there two potential paths to explore:
1. Simply use $\hat{\beta}_{OLS}$ - which doesn’t require a weight matrix, and come up with standard error estimates that are **robust to heteroskedasticity**
	- While coefficient estimates will no longer be optimal in Gauss-Markov sense, this will allow us to nonetheless conduct inference
2. Try to estimate $\text{Var}(\varepsilon)$ using the residuals from ordinary least squares; then, **use the inverse of the estimated variances** as weights
	- Called **feasible weighted least squares** (FWLS)
	- *Won't discuss in detail*

Under homoskedasticity, we based standard errors off of $$\hat{V} (\hat{\beta})=\hat{\sigma}_{\varepsilon}^{2}(X^{T}X)^{-1}$$ which was based upon the fact that under homoskedasticity, $\text{Var}(\hat{\beta})=\sigma_{\varepsilon}^{2}(X^{T}X)^{-1}$. 

Under heteroskedasticity, this variance estimate is generally **inconsistent** (even under favorable conditions). As $n \to \infty$ $$n\{\hat{V} (\hat{\beta})-\text{Var}(\hat{\beta})\}
\xrightarrow[]{p} C\ne 0$$
The notation $\xrightarrow[]{p}$ means **convergence in probability**. It’s a notion of convergence used for random variables.
- For a good statistical estimator, we want the difference between the estimate and the truth to **tend to zero**; an estimator with this property is called **consistent**
- Difference is not equal to zero above is troublesome

Now, consider the heteroskedastic generative model. Suppose (unrealistically) that I knew the value of $\varepsilon_{i}$ , and consider the random variable $\varepsilon_{i}^{2}$: $$\begin{align}
\mathbb{E}(\varepsilon_{i}^{2})&=\text{Var}(\varepsilon)+\mathbb{E}(\varepsilon_{i})^{2}\\
&= \text{Var}(\varepsilon)\\
&= \sigma^{2}_{\varepsilon}/w_{ii}
\end{align}$$
And, let $\text{diag}[\varepsilon_{i}^{2}]$ be an $n \times n$ diagonal matrix with $\varepsilon_{i}^{2}$ on the $i$ th diagonal, and consider the variance estimator $$(X^{T}X)^{-1}X^{T}\text{diag}[\varepsilon_{i}^{2}]X(X^{T}X)^{-1}$$
The proposed variance estimator happens to be an unbiased estimator for $\text{Var}(\hat{\beta})$, even **under heteroskedasticity**. $$\begin{align}
&\mathbb{E}[(X^{T}X)^{-1}X^T \text{diag}[\varepsilon_{i}^{2}]X(X^{T}X)^{-1}]\\
&= (X^{T}X)^{-1}X^T \mathbb{E}[\text{diag}[\varepsilon_{i}^{2}]] X(X^{T}X)^{-1}\\
&= (X^{T}X)^{-1}X^T \text{diag}[\sigma_{\varepsilon}^{2}/w_{ii}]X(X^{T}X)^{-1}\\
&= (X^{T}X)^{-1}X^T \sigma_{\varepsilon}^{2}W^{-1}X(X^{T}X)^{-1}\\
&= \text{Var}(\hat{\beta})
\end{align}$$
Regrettably we can’t use this estimator because it depends on $\varepsilon$. Instead, we’ll use a similar estimator which replaces $\varepsilon$ with a normalized version of $e=(I-H)y$, the ordinary least squares residuals.

```ad-important
**Definition 13.6**: Heteroskedasticity-consistent (HC) Variance Estimator

$$\hat{V} (\hat{\beta})_{HC2}=(X^{T}X)^{-1}X^T \text{diag}[e_{i}^{2}/(1-h_{ii})]X(X^{T}X)^{-1}$$

where $h_{ii}$ is the $ii$ diagonal of $H$ ([[10 Outliers#^94e388|leverage]]). Under homoskedasticity, we have $$\mathbb{E}(e_{i}^{2}/(1-h_{ii}))=\text{Var}\left(e_{i}/\sqrt{(1-h_{ii})}\right)=\sigma_{\varepsilon}^{2}$$
```

```ad-note
**Proof of Definition 13.6**

$$\begin{align}
\mathbb{E}(e_{i}^{2}/(1-h_{ii}))&=\text{Var}\left(e_{i}/\sqrt{(1-h_{ii})}\right)+\mathbb{E}\left(e_{i}/\sqrt{(1-h_{ii})}\right)^{2}\\
&= \text{Var}\left(e_{i}/\sqrt{(1-h_{ii})}\right)&(\mathbb{E}(e_{i})=0)\\
&= \sigma_{\varepsilon}^{2} &\text{( Above Definition 9.6 )}
\end{align}$$
```

Under *suitable conditions*, this variance estimator is **consistent** even if the truth is heteroskedastic:  $$n\{\hat{V}_{HC2} (\hat{\beta})-\text{Var}(\hat{\beta})\}
\xrightarrow[]{p}  0$$
We may also see this estimator referred to as a **sandwich estimator**.
- $(X^{T}X)^{-1}X^{T}$ is the bread
- $\text{diag}[e_{i}^{2}/(1-h_{ii})]$ is the meat

```ad-important
**Definition 13.7**: Heteroskedasticity-Consistent Standard Errors

The square roots of the diagonal elements of $\hat{V} (\hat{\beta})_{HC2}$ provide alternative standard errors for $\hat{\beta}$ that are consistent under heteroskedasticity: $$\text{se}_{HC2}(\hat{\beta}_{j})=\sqrt{\hat{V}_{HC2} (\hat{\beta})_{(j+1),(j+1)}}$$
```

These standard errors are valid **regardless of whether or not the truth is homoskedastic**.
- If the truth actually is actually homoskedastic, the **conventional standard errors perform better**
- But rarely is one certain that the truth is homoskedastic

There are *other* standard errors that are consistent under heteroskedasticity - hence the number "2".

```ad-example
**Example**: Comparing Confidence Intervals

![[Pasted image 20241019210920.png|400]]

Note that HC standard errors are much **larger**.

Given clear indication of heteroskedasticity, we should only trust the confidence intervals using HC standard errors.
- The usual standard errors assuming homoskedasticity would likely provide confidence intervals with coverage much lower than $95\%$
```

---
### Covered `R` Functions

```r
library(sandwich) # HC Standard Errors

# OLS Fit
ols.supervisors = lm(supervisors~workers)

# Hat Variance / Standard Errors
Vhat.HC2 = vcovHC(ols.supervisors, type = "HC2")
se.HC2 = sqrt(diag(Vhat.HC2))

# OLS Standard Errors
summary(ols.supervisors)$coef[,2]

# WLS Regression
lm(supervisors~workers, weights = 1/workers^2)
```
