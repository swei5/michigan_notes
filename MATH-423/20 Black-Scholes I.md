[[2024-11-12]] #Valuation #Derivatives 

### From Binomial Tree to Continuous time Model
Now we've studied a discrete time model (Binomial Tree Model), we would like to set up a continuous time model to describe the evolution of a stock price $S$ over a period of time, say $1$ year. We first divide a calendar year into $N$ intervals with equal length: $$\tau_{N}:=\frac{1}{N}$$
Consider a **binomial model**, for the stock $S$, on the time grid $$\{0,\frac{1}{N},\frac{2}{N},\cdots,\frac{N}{N}\}=\{0,\tau_{N},2\tau_{N},\cdots, N\tau_{N}=1\}$$
We will denote the the **one-step return** over the time interval $[(m-1)\tau_{N},m\tau_{N}]$ by $K_{N}(m)$, for every $m=1,2,\cdots, N-1, N$.

Since we have assumed a binomial model, the sequence $(K_{N}(m))_{m=1,\cdots,N}$ is i.i.d with $$K_{N}(1)=\begin{cases}
u_{N} \text{ with probability }p \\
d_{N} \text{ with probability }(1-p) \\
\end{cases}$$ For simplicity we will assume that $p=1$.

Let us rewrite the binomial model in a more *convenient* form. For $t=\frac{m}{N}=m\tau_{N}$: $$\begin{align}
S_{N}(t)=S_{N}(m\tau_{N})&=S_{N}((m-1)\tau_{N})(1+K_{N}(m))\\
&= S_{N}((m-2)\tau_{N})(1+K_{N}(m-1))(1+K_{N}(m))\\
&= S_{N}(0) \prod\limits_{i=1}^{N}(1+K_{N}(j)) & S_{N}(0)  \text{ is deterministic}\\
&= S_{N}(0) \exp\left[\ln \left( \prod\limits_{i=1}^{N}(1+K_{N}(j)\right)\right] & S_{N}(0))\\
&= S_{N}(0) \exp\left[\sum\limits_{j=1}^{n}k_{N}(j)\right]
\end{align}$$ where $k_{N}(j)=\ln (1+K_{N}(j))=\ln\left(\frac{S_{N}(j\tau_N)}{S_{N}((j-1)\tau_{N})}\right)$ is the **one step log return** of the stock at time $j$.

We denote the **one-year return** by $K$, which satisfies $$1+K=\prod\limits_{i=1}^{N}(1+K_{N}(j)) $$ and the **one-year log return** by $k$, which satisfies $$k=\sum\limits_{j=1}^{n}k_{N}(j)$$

We denote the **continuously compounded interest rate** by $r$. Then, the per $\tau_{N}$ -period compounded interest rate $r_{N}$ satisfies $$e^{r}=(1+r_{N})^{N}$$
The no-arbitrage principle can be written as $$d_N<r_{N}<u_{N} \iff 1+d_{N}<e^{r\tau_{N}}<1+u_{N}$$

```ad-important
**Definition 20.1**: Volatility of a Stock

The **volatility** of a **STOCK** over $1$ year is defined to be the **standard deviation of the log return** of the stock price in $1$ year.

In addition, if we assume the volatility over $1$ year is $\sigma$. Then, the volatility over a period of length $\tau_{N}$ is $\sigma \sqrt{\tau_{N}}$.
```

This is true since $$\begin{align}
\sigma^{2}=\text{Var}[k] &= \text{Var}\left[\sum\limits_{j=1}^{n}k_{N}(j)\right]\\
&= \sum\limits_{j=1}^{n}\text{Var}[k_{N}(j)] & k_{N}\text{ is independent}\\
&= N \text{Var}[k_{N}(1)] & k_{N}\text{ is identically distributed}\\
&= N \frac{\sigma^{2}}{N}
\end{align}$$ Thus, $$\sigma_{N}=\sqrt{\text{Var}[k_{N}(1)]}=\sqrt{\frac{\sigma^{2}}{N}}=\sigma\sqrt{\tau_N}$$

```ad-note
One can empirically estimate the volatility over a period, say over $[t, t+n(\Delta t)]$ where $\Delta t$ is a fixed time unit (e.g., day), by collecting historical data is the following:
- The stock price $S_{j}$ at the time $t+j\Delta t$ for $j=0, \cdots, N$
- Determine $u_{j}:=\ln\left(\frac{S_{j}}{S_{j-1}}\right)$ - which can be viewed as a **sample of log returns** $k_{N}(j)$ with $\tau_{N}=\Delta t$

Then, we can use the estimator for the standard deviation of $u_{j}$: $$s:=\sqrt{\frac{1}{n-1} \sum\limits_{i=1}^{n} (u_{j}-\bar{u})^{2}}$$ where $\bar{u}$ is the sample mean of $u$. Therefore, $$\hat{\sigma}=\frac{s}{\sqrt{\Delta t}}$$
```

#### Expectations, Variance of Log Returns
Now assume that we have made some analysis and we have estimated that $$\mathbb{E}(k)=\mu$$ and $$\text{Var}(k)=\sigma^{2}$$ Then, given the previous discussion, we have immediately that $$\mathbb{E}(k_{N}(1))=\mu\tau_{N}$$ and $$\text{Var}(k_{N}(1))=\sigma^{2}\tau_{N}$$
The **one-step log return** under the binomial tree model can be expressed as $$\ln (1+u_{N})=\mu \tau_{N}+\sigma \sqrt{\tau_{N}}$$ and $$\ln (1+u_{d})=\mu \tau_{N}-\sigma \sqrt{\tau_{N}}$$

```ad-note
**Proof**

We have that $$\begin{align}
\mathbb{E}(k_{N}(1)) &= \mathbb{E}(\ln(1+K_{N}(1))) \\
&= \frac{1}{2} \ln (1+u_{N})+ \frac{1}{2}(1+d_{N}) & \text{ assume } p_{u}=p_{d}  \\ 
&= \mu \tau_{N}
\end{align}$$
And $$\begin{align}
\text{Var}(k_{N}(1)) &= \mathbb{E}((k_{N}(1))^{2}) - (\mathbb{E}(k_{N}(1)))^{2}\\
&= \frac{1}{2}(\ln(1+u_{N}))^{2} + \frac{1}{2}(\ln(1+d_{N}))^{2} + (\mu \tau_{N})^{2}
\end{align}$$

Solving both equations simultaneously yields $$\begin{cases}
\ln (1+u_{N})=\mu \tau_{N}+\sigma \sqrt{\tau_{N}} = \mu \tau_{N} + \sigma_{N} \\
\ln (1+d_{N})=\mu \tau_{N}-\sigma \sqrt{\tau_{N}} = \mu \tau_{N} - \sigma_{N}
\end{cases}$$
```

^37217c

---
### Random Walk
Let us assume that we have an i.i.d. sequence of random variables $(\xi(j))_{j\in \mathbb{N}}$ such that $$\xi(1)=\begin{cases}
+1 & \text{ with probability } \frac{1}{2} \\
-1 & \text{ with probability } \frac{1}{2}
\end{cases}$$ 
```ad-important
**Definition 20.2**: Scaled Random Walk

We define the **stochastic process** $w$, called the **Random Walk**, by $$w(n):=\sum\limits_{j=1}^{n}\xi(j)$$ for all $n \in \mathbb{N}$ and the **scaled Random Walk** $$w_{N}(n\tau_{N}):=\frac{1}{\sqrt{N}}w(n)=\frac{1}{\sqrt{N}}\sum\limits_{j=1}^{n}\xi(j)= \sqrt{\tau_{N}} \sum\limits_{j=1}^{n}\xi(j)$$
```

```ad-note
The random walk *jumps* at every time unit by $\pm 1$. The scaled random walk *jumps* at every $\tau_N$-time unit, $\tau_{N}=\frac{1}{N}$ by $\pm \frac{1}{\sqrt{N}}$.
```

The random walk is a **martingale**, i.e. for every $n \in \mathbb{N}$, $$\mathbb{E}[w(n+1)|\xi(1), \cdots, \xi(n)]=w(n)$$ where $\mathcal{F}_{n}$ is the information one has up to time $n$.

```ad-note
**Proof of the Martingale**

$$\begin{align}
\mathbb{E}[w(n+1)|\xi(1), \cdots, \xi(n)]&=w(n) \\
&= \mathbb{E}\left[\sum\limits_{i=1}^{n+1} \xi(i)|\xi(1), \cdots, \xi(n)\right]\\
&= \mathbb{E}\left[\sum\limits_{i=1}^{n} \xi(i) + \xi(n+1)|\xi(1), \cdots, \xi(n)\right]\\
&= w(n) + \mathbb{E}[\xi(n+1)|\xi(1), \cdots, \xi(n)]\\
&= w(n) + \mathbb{E}[\xi(n+1)] & \text{ future observation independent of observed}\\
&= w(n)
\end{align}$$
```

![[Pasted image 20241112140333.png|500]]

If we scale the path to the extreme, we will start to see the path starting to *smooth out*.

```ad-important
**Definition 20.3**: Properties of the Expectation and Variance of Random Walk

We have that $$\mathbb{E}[w_{N}(n\tau_{N})]=0$$ and $$\text{Var}[w_{N}(n\tau_{N})]=\frac{n}{N}$$ for every $n \in \mathbb{N}$

In particular, $\text{Var}[w_{N}(1)] =1$ for every $N$.
```

As we have [[#^37217c|previously shown]], $$\begin{align}
k_{N}(j)&=\begin{cases}
\ln(1+u_{N}) & p=\frac{1}{2}  \\
\ln(1+d_{N}) & p=\frac{1}{2}
\end{cases} = \begin{cases}
\mu \tau_{N}+\sigma \sqrt{\tau_{N}}  \\
\mu \tau_{N}-\sigma \sqrt{\tau_{N}}
\end{cases}\\
&= \mu \tau_{N}+\sigma \xi(j)\sqrt{\tau_{N}}
\end{align}$$

```ad-important
**Definition 20.4**: Binomial Model with Random Walk

Given that $t=\frac{m}{N}=m\tau_{N}$, we may write the stock price at $t$ $S_{N}(t)$ as $$\begin{align}
S_{N}(t) &= S_{N}(0) \exp\left[\sum\limits_{j=1}^{m}(\mu \tau_{N}+\sigma \sqrt{\tau_{N}} \xi(j))\right]\\
&= S_{N}(0) \exp\left[m\mu\tau_{N}+\sigma \sqrt{\tau_{N}}\sum\limits_{j=1}^{m}(\xi(j))\right]\\
&= S_{N}(0) \exp\left[\mu t+\sigma w_{N}(t)\right] & m\tau_{N}=t, \text{ Definition 20.2}
\end{align}$$
```

^3cfe2f

Recall that we want to take the limit as $N \to \infty$ or equivalently as $\tau_{N} \to 0$. The above is effectively saying if we can determine $\lim_{N \to \infty}w_{N}(t)$ , we also have $\lim_{N \to \infty}S_{N}(t)$.

Since we have made use of the exponential, it is convenient to compare $S_{N}(t)$ and $S_{N}(t+\tau_{N})$. We know that the **second order approximation** of the exponential function is given by $$\exp(x)=\sum\limits_{k \in \mathbb{N}\cup\{0\}}\frac{x^{k}}{k!} \approx 1+x+ \frac{x^{2}}{2}$$ when $x$ is *very small*. Then, with $t=\frac{m}{N}, m\tau_{N}$, $$\begin{align}
\frac{S_{N}(t+\tau_{N})}{S_{N}(t)}&=\frac{S_{N}((m+1)\tau_{N})}{S_{N}(m\tau_{N})}=\exp(k_{N}(m+1))\\
&= \exp(\mu \tau_{N}+\sigma \xi(m+1)\sqrt{\tau_{N}})\\
&\approx 1+(\mu \tau_{N}+\sigma \xi(m+1)\sqrt{\tau_{N}}) +\frac{1}{2} \left(\mu \tau_{N}+\sigma \xi(m+1)\sqrt{\tau_{N}}\right)^{2} \\
&= 1+\mu \tau_{N}+\sigma \xi(m+1)\sqrt{\tau_{N}} + \frac{1}{2}\mu^{2}\tau_{N}^{2}+\mu\tau_{N}^{\frac{3}{2}}\sigma \xi(m+1)+ \frac{1}{2}\sigma^{2}\tau_{N} \xi^{2}(m+1)\\
&\approx 1+\left(\mu+ \frac{1}{2}\sigma^{2}\right)\tau_{N}+\sigma \sqrt{\tau_{N}} \xi(m+1) & \xi^{2}(\cdot) &= 1
\end{align}$$
As we get rid of higher order terms for a small $\tau_{N}$.  We can rewrite the approximation above as $$S_{N}(t+\tau_{N})-S_{N}(t)\approx S_{N}(t)\left(\mu+ \frac{1}{2}\sigma^{2}\right)\tau_{N}+S_{N}(t)\sigma [w_{N}(t+\tau_{N})-w_{N}(t)]$$
As we have $\xi (t+\tau_{N})=w_{N}(t+\tau_{N})-w_{N}(t)$ by definition.

```ad-important
**Definition 20.5**: The Black–Scholes model

The Central Limit Theorem will allow us to conclude the well-posedness of the **Stochastic Differential Equation** (SDE): $$dS(t)= S(t)\left(\mu+ \frac{1}{2}\sigma^{2}\right)dt + \sigma S(t)dW(t)$$
where $dS(t)=S(t+dt)-S(t)$ and $dW(t)=W(t+dt)-W(t)$.

The derivation of this is seen [[21 Black-Scholes II#^7a3abc|here]] later.
```

^9c7323

```ad-note
The SDE above is the **Black-Scholes Model**. It has a unique solution $$S(t)=S(0) \exp(\mu t+\sigma W(t))$$ where $W$ is the Brownian Motion.

Observe that for every $t \ge 0$, $$\ln\left(\frac{S(t)}{S(0)}\right)=\mu t+\sigma W(t) \sim \mathcal{N}(\mu t, \sigma^{2}t)$$

Here, the value $\sigma$ is the **volatility** of our price model. Under different probability measures that one is interested in, $\mu$ has different values and $W(t)$ takes different forms. However, the volatility $\sigma$ remains the **same** among all probability measures. The underlying reason of this fact, [Girsanov’s Theorem](https://en.wikipedia.org/wiki/Girsanov_theorem).
```

```ad-important
**Definition 20.6**: Donsker’s Theorem

The limit $W$ of the sequence of the scaled random walks $(w_{N})_{n \in \mathbb{N}}$ exits and it is a process with the following properties:
1. $W_{0}=0$
2. For every choice $0 \le s \le t \le u$, the increments $W(t)-W(s)$ and $W(u) - W(t)$ are **independent random variables**
3. For every choice $0 \le s \le t$ the increment $$W(t)-W(s) \sim \mathcal{N}(0,t-s)$$
4. The paths of $W(t)$ are **continuous** in $t$
```

^50b971

![[Pasted image 20241114151034.png|400]]

The paths of the limit process $W$ are *similar*.

```ad-note
The process $W$ described in Donsker’s Theorem is called **Brownian Motion**.
```

```ad-important
**Definition 20.7**: Properties of Black–Scholes model

Assume that $\mu^{\star}$ is the expected log return associated with the **risk neutral probability measure** $\mathbb{P}^{\star}$. Then $$\mu^{\star}=r- \frac{1}{2}\sigma^{2}$$

And, it can be proven that the **martingale measure** $\mathbb{P}^{\star}$ **exists and is unique**.
```
