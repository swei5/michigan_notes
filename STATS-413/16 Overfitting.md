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
As we add more polynomial terms, we are increasing $R^{2}$ by reducing the sum of squared residuals