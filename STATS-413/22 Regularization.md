[[2024-11-19]] #Regression #R 

### Regularization, Recap
We have derived the following optimization problem last lecture: $$\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)^{2}+\lambda \sum\limits_{j=1}^{p} |\tilde{\beta_{j}}|^{q}, q >0$$ For the above to be a sensible criterion to minimize, we need to standardize / $z$ -score the predictor variables $x_{1},\cdots, x_{p}$.
- Slope coefficients $\beta_{j}$ depend on the units of $x_{j}$
- Without further adjustment, the above would penalize the predictor with a **higher magnitude** than others (penalty depending on units)
	- Would also result in different degrees of penalization across predictor

Hence, when running regularized regression with $q>0$, we perform a $z$ -score transformation on each predictor variable $$z(x_{ij})=\frac{x_{ij}-\bar{x}_{j}}{\text{stdev}(x_{j})}$$ where $\bar{x}_{j}$ and $\text{stdev}(x_{j})$ are the sample mean and standard deviation for $x_{ij}$. This allows all the predictors to be on the same scale. See [[1 Simple Regression#$z$ -Score|Lecture 1]] for more information.

---
### Ridge Regression
We begin with ridge regression $q=2$. This replaces the ordinary least squares problem with the following: $$\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}\left(y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)\right)^{2}+\lambda \sum\limits_{j=1}^{p} |\tilde{\beta_{j}}|^{2}$$
When $\lambda \to 0$, this is an OLS with all variables. When $\lambda \to \infty$, the magnitude of slope coefficients $\hat{\beta}_{j}$ eventually shrinks towards zero.
- For this reason, regularized regression methods with $q>0$ are sometimes referred to as shrinkage methods

Ridge regression is particularly useful when $X^{T}X$ is not invertible, or when **strong multicollinearity** is present.

Let $X$ be an $n \times p$ design matrix with the intercept removed (so the first column is $x_{1}$, second column $x_{2}$, etc). Recall that we’ve standardized our predictor variables so that they have mean zero, standard deviation one.

```ad-important
**Definition 22.1**: Ridge Regression

For a given $\lambda$, the ridge regression problem yields slope coefficients $(\hat{\beta}^{R}_{\lambda 0}, \hat{\beta}^{R}_{\lambda})^{T}$ on the form $$\begin{align}
\hat{\beta}^{R}_{\lambda 0} &= \bar{y}\\
\hat{\beta}^{R}_{\lambda} &= (X^{T}X+\lambda I)^{-1}X^{T}y
\end{align}$$ where $I$ is the $p \times p$ identity matrix.
```

```ad-note
Even if $(X^{T}X)^{-1}$ is not invertible, $(X^{T}X+\lambda I)$ will be and is more stable than $(X^{T}X)^{-1}$. This is particularly useful under multicollinearity.
```

Let’s investigate the expectation and variance of the ridge coefficients.

```ad-important
**Definition 22.2**: Expectation and Variance of Ridge Regression

Suppose the weaker linear model holds, and let $\beta = (\beta_{1},\cdots, \beta_{p})$. $$\begin{align}
\mathbb{E}(\hat{\beta}^{R}_{\lambda}) &= (X^{T}X+\lambda I)^{-1}X^{T}X \beta\\
\text{Var}((\hat{\beta}^{R}_{\lambda}) &= \sigma^{2}_{\varepsilon}(X^{T}X+\lambda I)^{-1} X^{T}X (X^{T}X+\lambda I)^{-1}
\end{align}$$
```

The expectation states that unless $\lambda = 0$, $\hat{\beta}^{R}_{\lambda}$ is a **biased estimator** of $\beta$.
- For $\lambda > 0$, its variance is **ALWAYS SMALLER** than that of the OLS solution $\text{Var}(\hat{\beta})=\sigma_{\varepsilon}^{2}(X^{T}X)^{-1}$

Ridge regression thus sacrifices unbiasedness in exchange for stability. It trades off **increased bias for reduced variance**.

Recall that [[13 Heteroskedasticity#^8fffb5|Gauss-Markov Theorem]] says that among linear, unbiased estimators, $\hat{\beta}_{OLS}$ has the **smallest variance** under the weaker linear model.
- A corollary of this is among unbiased, linear estimators, $\hat{\beta}_{OLS}$ has the **smallest prediction error** under homoskedasticity and linearity

As we know, when forming predictions for $y^{\star}$ with covariates $\tilde{x}$: $$\text{Prediction Error}=\text{Irreducible Error} + \text{Squared Bias} + \text{Variance}$$
Gauss-Markov, however, does not rule out the existence of **biased estimators** with better prediction error than $\hat{\beta}_{OLS}$.
- Regularized regression methods try to accomplish this: **exchange increased squared bias** (no longer $0$) for **reduced variance** in a way that improves prediction error
- Especially beneficial under multicollinearity, or when $n \approx p$

```ad-note
The graph below shows black: bias of ridge regression; green: variance of ridge regression; purple: prediction error.

![[Pasted image 20241121010407.png|500]]

Increasing $\lambda$ increases bias but decreases variance.
```

In `R`, the function `cv.glmnet` within the `glmnet` package runs ridge regression, with $\lambda$ chosen based upon $10$ -fold cross-validation. If `x.train` contains our predictors and `y.train` contains our responses:

```r
ridge <- cv.glmnet(as.matrix(x.train), y.train, alpha = 0)
coefficient.ridge <- coef(ridge, s = "lambda.min")
predict.ridge <- predict(ridge, newx = as.matrix(x.test), s = "lambda.min")
```

`alpha=0` is how we tell the function we want **ridge regression**, and `s="lambda.min"` is how you tell it to use $\lambda$ equal to the cross-validated $\lambda$ for prediction equation.

```ad-note
By default, `glmnet` **standardizes** our variables, then runs ridge, then converts back to original units of $x$.
```

```ad-summary
**Benefits and Limitations of Ridge Regression**

**Pros**
- Computationally, ridge regression is a breeze to first
- No harder than ordinary least squares, and has extra **numerical stability**
- With $\lambda$ chosen via cross-validation, can improve out-of-sample performance relative to OLS using all variables
	- Effective tradeoff of bias and variance

**Cons**
- Will **NEVER** actually set a coefficient equal to zero
	- Resulting prediction equation will always be based on all $p$ coefficeints
	- In that sense, it isn't really performing a **model selection**
```

---
### LASSO Regression
LASSO (Least Absolute Shrinkage and Selection Operator) regression replaces the $\ell_{2}$ penalty of ridge with an $\ell_{1}$ penalty on the magnitudes of the slope coefficients: $$\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}\left(y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)\right)^{2}+\lambda \sum\limits_{j=1}^{p} |\tilde{\beta_{j}}|$$
Unlike with ridge regression, there’s not generally a closed-form solution for the LASSO coefficients.
- Still convex, so we can find the globally optimal solution using convex optimization algorithms

As with ridge regression, 
- The lasso coefficients, $\hat{\beta}^{L}_{\lambda}$, are biased estimates of $\beta$ for $\lambda >0$
- LASSO tries to trade off increased bias for improved variance/stability to improve overall prediction error
- Cross-validation is essential for navigating this trade-off

Unlike with Ridge Regression, LASSO can set slope coefficients **equal to zero**. Resulting prediction equation much more **interpretable**.

![[Pasted image 20241121012404.png|300]]

```ad-note
Why does LASSO perform variable selection, whereas ridge does not? In short
- $|\tilde{\beta}_{j}|^{2}=\tilde{\beta}_{j}^{2}$ is **differentiable** at $\beta_{j}=0$
- $|\tilde{\beta}_{j}|$ is **NOT differentiable** at $\beta_{j}=0$

For more intuition, we can equivalently write LASSO and Ridge in the following form for $q=1$ and $q=2$ respectively: $$\arg \min \limits_\tilde{\beta}\sum\limits_{i=1}^{n}\left(y_{i}-\left(\tilde{\beta_0}+\sum\limits_{j=1}^{p}\tilde{\beta}_{j}x_{ij}\right)\right)^{2}$$ subject to $$\sum\limits_{j=1}^{p} |\tilde{\beta_{j}}|^{q} \le C_{\lambda}$$

Equivalence: For every $\lambda$ in the original formulation, there exists a $C_{\lambda}$ so that the two formulation return **identical slope coefficients**.
```

The plot shows contours of equal RSS in red, along with the constraint sets in blue for LASSO (left) and ridge (right).
- Due to sharp corners of LASSO constraint set, intersection can occur when a collection of coefficients equal zero

![[Pasted image 20241121013521.png|400]]

To run LASSO in `R`, 

```r
lasso <- cv.glmnet(as.matrix(x.train), y.train, alpha = 1)
coefficient.lasso <- coef(lasso, s = "lambda.min")
predict.lasso <- predict(lasso, newx = as.matrix(x.test), s = "lambda.min")
```

We set `alpha=1` (which is the default parameter value too) instead.

---
### Regression in High Dimensions
When we introduced ordinary least squares, we required that $(X^{T}X)$ was invertible.
- Fails under perfect [[11 Collinearity|multicollinearity]]
- Fails if $p \ge n$ (more predictors than observations)

In some domains, it is common to have $p >> n$. Sometimes, *big data* means **many features** - large number of columns, limited number of rows.

A general assumption in situations with $p >> n$ is that a large number of the covariates have $\beta_{j}$ equal to zero.
- Many candidate predictors, but we only believe **a few of them are important**
- This is called a **sparse regression context**

To put this into context, consider the following system of equations: $$y=X\hat{\beta}$$
For $p \ge n-1$, if the rows of $X$ are **linearly independent**, we can **perfectly fit** our observed data, producing an in-sample RSS equal to zero.
- If $p+1 = n$, we can find a **single value** for $\hat{\beta}$ that perfectly reproduces the observed responses
- If $p \ge n$, there are generally **infinitely many values** of $\hat{\beta}$ that perfectly reproduce the observed responses

This is an extreme example of **overfitting**.

In high-dimensional settings, LASSO has particular appeal.
- Still feasible computationally to run with $p$ in thousands/millions
- By penalizing complexity, avoid overfitting
- LASSO can set many coefficients to zero - performing model selection

---
### Inference after LASSO
In general, developing methods inference on slope coefficients after running LASSO remains an open area of research.
- The bias in LASSO coefficients and the problem's geometry add some challenging theoretical hurdles to overcome

One valid approach can be accomplished through **sample splitting**.
- Use LASSO with on the training set to select a small set of nonzero coefficients for covariates $x_{j}$
- Run OLS on the test set, using **ONLY the covariates** that LASSO assigned nonzero slope coefficients
- Use the usual tools for inference for OLS on the test set

In other words, the LASSO on the training set takes an exploratory role. OLS on the test set is then confirmatory.
