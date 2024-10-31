[[2024-10-29]] #Regression 

As we know by now, a key assumption underpinning multiple regression is the assumption of **linearity**. In practice, we might instead believe $$\mathbb{E}(y|x)=f(x_1,\dots,x_{p})$$ such that our real data are noisy observations from some complicated, **nonlinear**, **multivariate** function $f(\cdot)$. 

If there’s a particular transformation of $y$ or $x$ that is known/suspected to make relationship linear, apply it (e.g. log transformation).

What if we don't know $f(\cdot)$? We can try to approximate.

```ad-important
**Definition 16.1**: Taylor Series

A function $f$ **infinitely differentiable** at a point a can be expressed as $$f(x)=\sum\limits_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!}(x-a)^{n}$$ where $f^{(n)}(\cdot)$ is the $n$th derivative of $f(\cdot)$.
```

Setting $a=0$ and letting $\beta_{j}=\frac{f^{(j)}(0)}{j!}$ gives $$\begin{align}
f(x) &= \beta_{0}+\beta_{1}x+\beta_{2}x^{2}+\cdots \\
&= \sum\limits_{j=0}^{\infty}\beta_{j}x^{j}
\end{align}$$
Truncating the infinite sum after p polynomials, we may instead consider $$f(x)\approx \sum\limits_{j=0}^{p}\beta_{j}x^{j}$$ That is, we can approximate $f(x)$ through a **sum of polynomials of increasing degrees**.

A reasonable approach for finding the best approximating polynomial of degree $p$ would be to run ordinary least squares: $$\min\limits_{\beta}\sum\limits_{i=1}^{n}[y_{i}-(\beta_{0}+\beta_{1}x_{i}+\beta_{2}x_{i}^{2}+\cdots+\beta_{p}x_{i}^{p})]^{2}$$ 
We can create the transformed covariates in `R` of the form $x_{j}=x^{j}$ by `lm(y~poly(x, degree=p, raw = T))`.

![[Pasted image 20241029152123.png|400]]

```ad-note
**Interpretations of Slope Coefficients**

We don’t ascribe interpretations to these coefficients but rather focus mainly on the **quality of the resulting fit**.

This is because we can't *hold all other variables fixed* - for example, if the units differ in $x^2$, then they also differ in $x$, $x^{3}$, and so on.
```

Also note that $R^2$ values increase as we increase the degree.

---
### Overfitting
As we add more polynomial terms, we are increasing $R^{2}$ by reducing the sum of squared residuals. Overfitting is the production of an analysis that corresponds too **closely**, or even exactly, to a particular set of data.

By corresponding too closely, or even exactly, to a particular set of data, the analysis may **fail to generalize** to additional data, and may fail to predict future observations reliably. There’s a tradeoff between accounting for true nonlinearities and fitting too closely to the data at hand.


```ad-example
**Example**: Tradeoff and RMSE

We start with a data set of size 50 generated from this function and fit polynomials $j$ of varying degrees to our observed data set using least squares $\hat{f}_{j}$. Then, for each **new** observation, we compute the RMSE of our prediction: $$RMSE(j)=\sqrt{\frac{1}{n}\sum\limits_{i=1}^{n}(\hat{f}_{j}(x_{i})-y_{i}^{\star})^{2}}$$

![[Pasted image 20241030125304.png|400]]

Smaller Values for RMSE means we did better at predicting future observations.
```

The risk of overfitting is present with many statistical methods, and might occur if we
- Include **irrelevantvant** predictor variables
- Include **unnecessary** interaction terms
- Allow our functional approximation to be too **flexible**

This highlights the deficiency of measures such as $R^{2}$ for assessing model performance.
- $R^{2}$ always improves with model complexity **in-sample**, but that doesn’t reflect predictive accuracy for future observations (often called **out-of-sample performance**)

---
### Prediction Errors
The previous illustration of overfitting simulated multiple data sets, but of course we’re stuck with a single data set.

In general, suppose our data arise through $$y_{i}=f(x_{i})+\varepsilon_{i}$$ where $\varepsilon$ are iid with $\mathbb{E}(\varepsilon)=0$ and $\text{Var}(\varepsilon)=\sigma_{\epsilon}^{2}$. Based on our data we come up with a predictive model $\hat{f}(x)$. Now, we consider using our prediction function from our data set $\hat{f}(x)$ to predict a **new**/**future** observation at a point $\tilde{x}$. Call the unknown future observation $y^{\star}$.

Our prediction, naturally, is what our model tells us given their $x$ value is $\hat{f}(\tilde{x})$. We expect $y^\star$ to be away from our our prediction $\hat{f}(x)$ by a distance of $$\mathbb{E}[(y^{\star}-\hat{f}(\tilde{x}))^{2}]=\mathbb{E}[(y^{\star}-{f}(\tilde{x}))^{2}]+\mathbb{E}[(\hat{f}(\tilde{x})-{f}(\tilde{x}))^{2}]$$ which we can break down into two components:
1. **Irreducible Error**: $\mathbb{E}[(y^{\star}-{f}(\tilde{x}))^{2}]$
	- Even if I knew the true function $f(\tilde{x})$, my responses have intrinsic randomness associated with them, $\sigma_{\varepsilon}^{2}$
	- There’s **nothing** we can do about this for a fixed set of covariates- inherent noise in this system
2. **Reducible Error**: $\mathbb{E}[(\hat{f}(\tilde{x})-{f}(\tilde{x}))^{2}]$
	- This error we can reduce, based on how we choose $\hat{f}$ based on our data

```ad-note
We’ve actually seen both of these errors before when forming [[8 Prediction Intervals|prediction intervals]]. Under the stronger linear model, $f(\tilde{x})=\tilde{x}^T \beta$ and our prediction intervals were based on the variance of $y^{\star}-\hat{\mu}_{y|\tilde{x}}=y^{\star}-\tilde{x}^T \hat{\beta}$ $$\begin{align}
\text{Var}(y^{\star}-\tilde{x}^{T} \hat{\beta}) &= \text{Var}(y^{\star})+\text{Var}(\tilde{x}^{T} \hat{\beta})\\
&= \mathbb{E}[(y^{\star}-\tilde{x}^{T}\beta)^{2}] + \mathbb{E}[(\tilde{x}^{T}\hat{\beta}-\tilde{x}^{T}\beta)^{2}]
\end{align}$$ where
- Irreducible error: $\text{Var}(y^{\star})=\sigma_{\varepsilon}^{2}$
- Reducible error: $\text{Var}(\tilde{x}^{T} \hat{\beta})=\sigma_{\varepsilon}^{2} \tilde{x}^{T}(X^{T}X)^{-1}\tilde{x}$
```

In thinking about how to reduce the **reducible error**, consider a further decomposition. $$\mathbb{E}[(\hat{f}(\tilde{x})-{f}(\tilde{x}))^{2}]=(\mathbb{E}[\hat{f}(\tilde{x})]-f(\tilde{x}))^{2}+\mathbb{E}[(\hat{f}(\tilde{x})-\mathbb{E}[\hat{f}(\tilde{x})])^{2}]$$ which we can also break down into two components:
1. **Estimator Bias** (squared): $(\mathbb{E}[\hat{f}(\tilde{x})]-f(\tilde{x}))^{2}$
	- Is there a systematic difference between $\mathbb{E}[\hat{f}(\tilde{x})]$ and $f(\tilde{x})$?
2. **Estimator Variance**: $\mathbb{E}[(\hat{f}(\tilde{x})-\mathbb{E}[\hat{f}(\tilde{x})])^{2}]$
	- How much does my predicted value $\hat{f}(\tilde{x})$ vary from one data set to the next?
	- *How stable is my prediction function?*

Bias refers to the error that is introduced by modeling a real life problem (that is usually extremely complicated) by a **much simpler problem**.
- The **more flexible/complex** a model is, the **LESS bias** it will generally have

Variance refers to how much your estimate for $f(\cdot)$, $\hat{f}(\cdot)$ would change if you had a different data set.
- Generally, the **more flexible** a model is, the **MORE variance** it has.

```ad-example
**Example**: Reducible Errors in Linear Regression

Decomposing reducible error for $\text{Var}(\tilde{x}^{T} \hat{\beta})=\sigma_{\varepsilon}^{2} \tilde{x}^{T}(X^{T}X)^{-1}\tilde{x}$:
- **Squared bias**: $0$ since $\mathbb{E}(\tilde{x}^{T}\hat{\beta})-\tilde{x}^{T}\beta=0$
- **Variance**: $\sigma_{\varepsilon}^{2} \tilde{x}^{T}(X^{T}X)^{-1}\tilde{x}$

Under the stronger linear model, if we fit a line, the **only source of reducible error** is the **variance of our prediction function**.
```

```ad-example
**Example: Fitting a Line When Truth is Quadratic**

![[Pasted image 20241030133427.png|500]]
```

---
### Bias-Variance Tradeoff
Prediction Error can thus be decomposed into three terms: $$\mathbb{E}[(y^{\star}-\hat{f}(\tilde{x}))^{2}]=\mathbb{E}[(y^{\star}-{f}(\tilde{x}))^{2}]+(\mathbb{E}[\hat{f}(\tilde{x})]-f(\tilde{x}))^{2}+\mathbb{E}[(\hat{f}(\tilde{x})-\mathbb{E}[\hat{f}(\tilde{x})])^{2}]$$ The last two terms are the terms that we can hope to control based on how we use our data to estimate $\hat{f}(\tilde{x})$.
- Ideally, we’d like to minimize both terms
- Unfortunately, strategies that minimize one term need not minimize the other

**Increasing the polynomial** degree **decreases bias** (we are getting better approximations of the true function) but also **increases** variance, as we begin to fit too much to noise.

This is a general phenomenon known as the **bias-variance tradeoff**.

```ad-important
**Definition 16.2**: The Bias-Variance Tradeoff

As a general rule, as we **increase** model complexity:
- Estimator bias **decreases**
- Estimator variance **increases**
```

Errors for future observations will decline at first (as reductions in bias dominate) but will then start to increase again (as increases in variance dominate)

![[Pasted image 20241030135513.png|400]]
