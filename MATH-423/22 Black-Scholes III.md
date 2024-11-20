[[2024-11-19]] #Valuation #Derivatives 

In Lecture 20 and 21 we mentioned that there exists a unique martingale measure for the Black–Scholes model.

We have also mentioned that it is the measure $\mathbb{P}^{\star}$ corresponding to $\mu^{\star}=r- \frac{1}{2}\sigma^{2}$ where $r$ is the continuously compounded interest rate in the market.

Under the $\mathbb{P}^{\star}$ measure (and by means of Girsanov’s theorem) we can rewrite the B-S Model as $$dS(t)= rS(t)dt + \sigma S(t)dW_{t}^{\star} \text{ under } \mathbb{P}^{\star}$$ where $W^{\star}$ is a process which is a Brownian Motion under $\mathbb{P}^{\star}$.

```ad-example
**Example**: Solution to the B-S SDE ($\mathbb{P}^{\star}$)

We want to show that the solution to the B-S SDE problem is $S(0)\exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma W_t\right]$ for every $t \in \mathbb{R}^{+}$. First, let $g(t,x):=S(0)\exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma x\right]$ and apply Itô's formula: $$\begin{align}
dg(t,x) &= S(0)\left((r-\frac{1}{2}\sigma^{2}) \exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma x\right]dt + \sigma \exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma x\right]dx + \frac{1}{2} \sigma^{2} \exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma x\right]dt  \right)\\
&= S(0)  \exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma x\right] \left(rdt + \sigma dx \right)\\
&= rg(t,x)dt + \sigma g(t,x) dt
\end{align}$$

which matches the original SDE.
```

```ad-example
**Example**: The discounted price process is a $\mathbb{P}^\star$-martingale

Similar to last example, we set $g(t,x):=S(0)\exp\left[- \frac{1}{2}\sigma^{2}t+\sigma x\right]$ and apply Itô to compute $dg(t,x)$. Specifically, we need to apply Itô’s formula to the process $$\tilde{S}(t)=e^{-rt}S(t)=S(0)\exp\left[- \frac{1}{2}\sigma^{2}t+\sigma W^{\star}_{t}\right]$$

Note that this was based on the fact that $S(t)=S(0)\exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma W_t\right]$ under $\mathbb{P}^{\star}$.

Repeating what we did in the first example allows us to get $$\begin{align}
\tilde{S}(t) &= \tilde{S}(0) + \int_{0}^{t} \frac{\partial g}{\partial s}(s, W^{\star}_{s}) ds + \int_{0}^{t} \frac{\partial g}{\partial x}(s, W^{\star}_{s}) dW^{\star}_{s} + \frac{1}{2} \int_{0}^{t} \frac{\partial^{2} g}{\partial x^{2}}(s, W^{\star}_{s}) ds\\
&= \tilde{S}(0) + \int_{0}^{t} \left[-\frac{1}{2} \sigma^{2} + \frac{1}{2} \sigma^{2} \right] g(s,W_{s}^{\star}) ds + \int_{0}^{t} \sigma g(s, W_{s}^{\star}) dW_{s}^{\star} \\
&= \tilde{S}(0) + \int_{0}^{t} \sigma g(s, W_{s}^{\star}) dW_{s}^{\star} 
\end{align}$$

Since the function $g(t,x)=S(0)\exp\left[- \frac{1}{2}\sigma^{2}t+\sigma x\right]$ grows at most as fast as an exponential function, the process consisting of the stochastic integrals $$\int_{0}^{t} \sigma g(s, W_{s}^{\star}) dW_{s}^{\star}$$ is an $\mathbb{F}^{W}$-martingale. ([[21 Black-Scholes II#^a962f7|Definition 21.2]])

Therefore, $tilde{S}(t) = \tilde{S}(0)+\int_{0}^{t} \sigma g(s, W_{s}^{\star}) dW_{s}^{\star}$ is an $\mathbb{F}^{W}$-martingale under the measure $\mathbb{P}^{\star}$.
```

```ad-important
**Definition 22.1**: Girsanov’s Theorem, Revisited

Girsanov’s Theorem describes the relationship between the processes $W$ and $W^{\star}$: $$W^{\star}_{t}=W_{t}+\frac{\mu - r + \frac{1}{2} \sigma^{2}}{\sigma} t$$

Thus, 
- The information $\mathbb{F}^{{W}^{\star}}$ and $\mathbb{F}^{W}$ are the same: if we know all the history of the process $W$ up to time $t$, then we also know what happened to the process $W^{\star}$ up to time tand vice versa
- The process $W$ is a Brownian Motion and martingale under the measure $\mathbb{P}$, but **NOT** under the measure $\mathbb{P}^{\star}$
- The process $W^{\star}$ is a Brownian Motion and martingale under the measure $\mathbb{P}^{\star}$, but **NOT** under the measure $\mathbb{P}$
```

---
### Pricing European Options

```ad-important
**Definition 22.2**: European Call under B-S Model

For a European call with strike price $X$ and exercise date $T$, the no arbitrage price of the option under the martingale measure $\mathbb{P}^{\star}$ is $$C_{E}(0)=\mathbb{E}^{\star}[e^{-rT}(S(T)-X)^{+}]$$

More generally, the price at time $t \in [0,T]$ is $$C_{E}(T)=\mathbb{E}^{\star}[e^{-r(T-t)}(S(T)-X)^{+}|\mathcal{F}_{t}^{W}]$$

The proof of this identity is beyond the scope of this course and omitted.
```
