[[2024-10-17]] #Regression 

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
\end{align}$$ where $\varepsilon_{1},\cdots, \varepsilon_{n}$ are independent, $W$ is a $n \times n$ diagonal matrix with $w_{ii}>0$ on the diagonals (zeros off the diagonals)
- Implies $W^{-1}$ is a $n \times n$ diagonal matrix with $1/w_{ii}$ on the diagonals (zeros off the diagonals)
- $W=W^{-1}=I$ implies homoskedasticity

Suppose the previous model for heteroskedasticity holds; then: $$\begin{align}
\mathbb{E}(\hat{\beta}) &= \beta\\
\text{Var}(\hat{\beta})&=\sigma_{\varepsilon}^{2}(X^TX)^{-1}X^{T}W^{-1}(X^TX)^{-1} \\
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
```

```ad-note
**Proof of Definition 13.4**

![[Pasted image 20241018175619.png|400]]

![[Pasted image 20241018175632.png|400]]
```

Note that if $W=W^{-1}=I$, $\hat{\beta}_{WLS}=\hat{\beta}_{OLS}$.
- Under homoskedasticity, $\hat{\beta}_{OLS}$ is the best linear unbiased estimator for $\beta$
- The derivation above provides additional justification for using OLS to estimate $\beta$ under the weaker and stronger linear model
- Any other linear, unbiased estimator would be worse than $\hat{\beta}_{OLS}$

