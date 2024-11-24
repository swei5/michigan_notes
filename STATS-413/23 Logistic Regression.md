[[2024-11-21]] #Regression #R

We are interested in regression with binary responses. Recall that multiple regression is an attempt to model the **conditional expectation** of $y_{i}$ given $x_{i}$. If linearity holds, $$\mathbb{E}(y_{i}|x_{i})=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}$$ Note that the expectation of a binary variable is equal to $$\mathbb{E}(y_i|x_i)=\mathbb{P}(y_{i}=1|x_{i})$$ which implies that the multiple regression model would take the form $$p(x_{i})=\mathbb{P}(y_{i}=1|x_{i})=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}$$ i.e. that the **probability of success is linear** in the covariates.

To visualize, let’s consider a simple regression example with a binary response. The line is the line of best fit.

![[Pasted image 20241123204631.png|500]]

This obviously isn't a good approximation. The predicted outcome may fall outside of the range of $0$ or $1$. 

Even if the probabilities were linear in the covariates, there are other issues when using linear regression to fit binary outcomes. Stronger linear model **CANNOT hold**
- $\varepsilon_{i}$ terms **CANNOT** be normally distributed if outcome is binary - this would imply **continuous responses**

In addition, if $y$ is Bernoulli with probability of success given $x$ of $p(x)$, then $$\text{Var}(y|x)=p(x)(1-p(x))$$
- **Homoskedasticity** also cannot hold!

While these issues could be remediated with [[13 Heteroskedasticity|HC standard errors]] and with the [[14 Non-Normality|bootstrap]], given the fundamental issues with linearity when predicting probabilities we take a different approach.

---
### Logistic Regression
The issue here is that directly fitting a linear model to binary data fails to account for the constraint that $0 \le p (x) \le 1$.

Our first idea is to instead of directly fitting a linear function for $p(x)$, we instead do so for a function of $p (x)$ that is unbounded. This is called a **link function**.

For regression with binary responses, by far the most common choice is the **logit link**: $$\text{logit}(p(x))=\log\left(\frac{p(x)}{1-p(x)}\right)$$ where $\text{logit}(p(x))$ is also known as the **log-odds**, as $\frac{p(x)}{1-p(x)}$ is the **odds** of observing $1$ given $x$.

We now imagine that a linear model holds on the logit scale: $$\text{logit}(p(x))=\log\left(\frac{p(x)}{1-p(x)}\right)=\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}$$ Solving for $p(x_{i})$ yields $$p(x_{i})=\frac{\exp(\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip})}{1+\exp(\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip})}$$ as in multiple regression, $\beta_{j}$ are unknown parameters of the model, governing the true probability of a $1$ given $x$.

```ad-important
**Definition 23.1**: Logistic Function

The **logistic** function is the function $$\text{logistic}(\theta)= \frac{\exp(\theta)}{1+\exp(\theta)}$$

And it has the following properties:
- $0 < \text{logistic}(\theta)<1$
- $\text{logistic}(0)= \frac{1}{2}$
- $\text{logistic}(\theta)$ is an **increasing function** of $\theta$

Let $\theta = \beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip}$
- If $\beta_{j} > 0$, then the function is increasing in $x_{i}$, and vice versa
```

We will now use the observed data to estimate the parameters $\beta_{0}, \cdots, \beta_{p}$. That is, we will use data to estimate $$p(x)=\frac{\exp(\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip})} {1+\exp(\beta_{0}+\beta_{1}x_{i1}+\cdots+\beta_{p}x_{ip})}$$ by means of $$\hat{p}(x)=\frac{\exp(\hat{\beta_{0}}+\hat{\beta_{1}}x_{i1}+\cdots+\hat{\beta}_{p}x_{ip})}{1+\exp(\hat{\beta_{0}}+\hat{\beta_{1}}x_{i1}+\cdots+\hat{\beta}_{p}x_{ip})}$$
The process of fitting the function $\hat{p}(x)$ is known as **logistic regression**.

In `R`, we can use the following commands: 

```r
glm(Osteo~Weight, family = "binomial", data = train)

predict(..., newdata = ...) # gives predicted log-odds by default
predict(..., newdata = ..., type = "response") # gives predicted probabilities
```

`family = "binomial"` signifies a logistic regression.

---
### Maximum Likelihood Estimation
How did `R` come up with this equation?
- In linear regression - `R` finds the line that minimizes the **in-sample RSS**
- But with **binary data**, we already went down that road and weren’t too pleased with the resulting prediction equation - it did not account for the fact that we are predicting probabilities

Instead, `R` comes up with this equation through a process known as **maximum likelihood estimation**.

Suppose I knew the values of $\beta_{0}, \beta_{1}, \cdots, \beta_{p}$. At any point $x_{i}=[1, x_{i1}, x_{ip}]^{T}$, what’s the probability of observing a $1$ given $x_{i}$?

$$\begin{cases}
\mathbb{P}(y_{i}=1|x_{i})=\frac{\exp(x_{i}^{T}\beta)}{1+\exp(x_{i}^{T}\beta)} \\
\mathbb{P}(y_{i}=0|x_{i})=\frac{1}{1+\exp(x_{i}^{T}\beta)}
\end{cases}$$ We can write this concisely as $$p(y_{i}|x_{i})=\left(\frac{\exp(x_{i}^{T}\beta)}{1+\exp(x_{i}^{T}\beta)}\right)^{y_{i}}\left(\frac{1}{1+\exp(x_{i}^{T}\beta)}\right)^{1-y_{i}}$$ as $y_{i}=1$ or $y_{i}=0$.

In our dataset, we have $n$ pairs $(x_{1}, y_{1}), \cdots, (x_{n},y_{n})$. We assume independence between observations. Then, given $x_{1}, x_{2},\cdots, x_{n}$, the probability of observing $y_{1}$ and $y_2$ and $\cdots$ and $y_{n}$ is $$\begin{align}
p(y_{1}\cap y_{2} \cap \cdots \cap y_{n}|x_{1}, \cdots, x_{n})&= \prod_{i=1}^{n}p(y_{i}|x_{i})\\
&= \prod_{i=1}^{n} \left(\frac{\exp(x_{i}^{T}\beta)}{1+\exp(x_{i}^{T}\beta)}\right)^{y_{i}}\left(\frac{1}{1+\exp(x_{i}^{T}\beta)}\right)^{1-y_{i}}
\end{align}$$
We know $\{y_{i}\}$ and $\{x_{i}\}$ from our data, but $\beta$ are unknown. This leads us to the following maximization problem. $$\hat{\beta}=\arg\max\limits_{\tilde{\beta}} \prod_{i=1}^{n} \left(\frac{\exp(x_{i}^{T}\tilde{\beta})}{1+\exp(x_{i}^{T}\tilde{\beta})}\right)^{y_{i}}\left(\frac{1}{1+\exp(x_{i}^{T}\tilde{\beta})}\right)^{1-y_{i}}$$ There’s no closed form solution, but `R` solves it numerically.