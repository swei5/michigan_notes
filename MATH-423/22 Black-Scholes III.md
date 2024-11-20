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

Note that this was based on the fact that $S(t)=S(0)\exp\left[(r- \frac{1}{2}\sigma^{2})t+\sigma W^\star_t\right]$ under $\mathbb{P}^{\star}$.

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
The density of the random variable $W^{\star}_{t}$ under the measure $\mathbb{P}^{\star}$ is given by $$f(t;x)=\frac{1}{\sqrt{2\pi t}} \exp\left[- \frac{x^{2}}{2t}\right]$$ Equivalently, $W^{\star}_{t} \sim \mathcal{N}(0, t)$ under the measure $\mathbb{P}^{\star}$. (Recall [[20 Black-Scholes I#^50b971|Definition 20.6]])

For a European call with strike price $X$ and exercise date $T$, the no arbitrage price of the option under the martingale measure $\mathbb{P}^{\star}$ is $$C_{E}(0)=\mathbb{E}^{\star}[e^{-rT}(S(T)-X)^{+}]$$

More generally, the price at time $t \in [0,T]$ is $$C_{E}(T)=\mathbb{E}^{\star}[e^{-r(T-t)}(S(T)-X)^{+}|\mathcal{F}_{t}^{W}]$$
The proof of this identity is beyond the scope of this course and omitted.

We can calculate the expectation with respect to the martingale measure $\mathbb{P}^{\star}$. We observe that $$(S(T)-X)^{+}=(S(T)-X) \mathbb{1}(S(T)>X)$$ and that $$\begin{align}
S(T) >X &\iff S(0)\exp\left[(r- \frac{1}{2}\sigma^{2})T+\sigma W^\star_T\right]>X\\
&\iff W_{T}^{\star} > \frac{\ln \left(\frac{X}{S(0)}\right)-(r- \frac{1}{2}\sigma^{2})T}{\sigma}\\
&\iff N >\frac{\ln \left(\frac{X}{S(0)}\right)-(r- \frac{1}{2}\sigma^{2})T}{\sigma \sqrt{T}}
\end{align}$$ where in the last equivalence we used $W^{\star}_{t} \sim \mathcal{N}(0,t)$ under the measure $\mathbb{P}^{\star}$. For notational convenience we define $$d_{\pm}= \frac{\ln \left(\frac{S(0)}{X}\right)-(r\pm \frac{1}{2}\sigma^{2})T}{\sigma \sqrt{T}}$$
Observe that we just worked on the event $\mathbb{P}(S (T)>X)=\mathbb{P}^{\star}(N>-d_{-})$. Now we can derive the price $$\begin{align}
C_{E}(0)&=\mathbb{E}^{\star}[e^{-rT}(S(T)-X)^{+}]\\
&= \mathbb{E}^{\star}[(S(T)e^{-rT}-Xe^{-rT}){\mathbb{1}_{(S(T)>X)}}]\\
&= \mathbb{E}^{\star}\left[S(0)\exp\left[- \frac{1}{2}\sigma^{2}T+\sigma \sqrt{T}N\right]-Xe^{-rT}{\mathbb{1}_{(N>-d_{-})}}\right]\\
&= \int_{-d_{-}}^{\infty} \left(S(0)\exp\left[- \frac{1}{2}\sigma^{2}T+\sigma \sqrt{T}x\right]-Xe^{-rT}\right) \frac{1}{\sqrt{2\pi }}\exp\left(- \frac{x^{2}}{2}\right) dx\\
&= \frac{S(0)}{\sqrt{2\pi}}  \int_{-d_{-}}^{\infty} \exp\left[- \frac{\sigma^{2}T-\sigma\sqrt{T}x+x^{2}}{2}\right]dx - \frac{Xe^{-rT}}{\sqrt{2\pi}} \int_{-d_{-}}^{\infty} \exp\left(- \frac{x^{2}}{2}\right)dx\\
&= \frac{S(0)}{\sqrt{2\pi}}  \int_{-d_{-}}^{\infty} \exp\left[- \frac{(x-\sigma \sqrt{T})^{2}}{2}\right]dx - Xe^{-rT} [1-\Phi(-d_{-})]
\end{align}$$ where $\Phi$ is the cumulative distribution function of the standard normal distribution.

In order to compute the first integral we will use the change of variable $y=x-\sigma \sqrt{T}$, for which we have $dy=dx$ and $$-d_{-}-\sigma\sqrt{T}=\frac{\ln \left(\frac{S(0)}{X}\right)+(r- \frac{1}{2}\sigma^{2})T}{\sigma \sqrt{T}}-\frac{\sigma^{2}T}{\sigma\sqrt{T}}=-d_{+}$$ therefore we have $$\begin{align}
C_{E}(0) &= \frac{S(0)}{\sqrt{2\pi}}  \int_{-d_{-}}^{\infty} \exp\left[- \frac{(x-\sigma \sqrt{T})^{2}}{2}\right]dx - Xe^{-rT} [1-\Phi(-d_{-})]\\
&= \frac{S(0)}{\sqrt{2\pi}}  \int_{-d_{+}}^{\infty} \exp\left[- \frac{y^{2}}{2}\right]dy - Xe^{-rT} \Phi(d_{-}) & \text{ symmetry of } \Phi\\ 
&= S(0) [1-\Phi(-d_{+})] -Xe^{-rT} \Phi(d_{-})\\
&= S(0) \Phi(d_{+}) -Xe^{-rT} \Phi(d_{-})
\end{align}$$
One can follow similar arguments for the arbitrary time $t$ and derive a result where the time $T$ is substituted by the remaining time $T-t$.

```ad-important
**Definition 22.2**: Black–Scholes formula for the European call

The price at time $t \in [0,T)$ of the European call with strike price $X$ and exercise time $T$ is given by $$C_{E}(t)=S(t) \Phi(d_{+}(t)) -Xe^{-r(T-t)} \Phi(d_{-}(t))$$ where $$d_{\pm}(t):=\frac{\ln \left(\frac{S(t)}{X}\right)-(r\pm \frac{1}{2}\sigma^{2})(T-t)}{\sigma \sqrt{T-t}}$$
```

Use Put–Call Parity, we may derive that the price at time $t$ of the European put with strike price $X$ and exercise time $T$ is given by $$P_{E}(t)=Xe^{-r(T-t)}\Phi(-d_{-}(t))-S(t)\Phi(-d_{+}(t))$$
In general, for a European option $D$ with payoff function $g (S (T))$ at the maturity time $T$, its value at $t \in [0,T]$ is given by $$D(T)=V(t,S(t))$$ where $V(t,x)$ is a **solution** to the Black–Scholes Partial Differential Equation $$\frac{\partial V}{\partial t}(t,x)+ \frac{1}{2}\sigma^{2} x^{2} \frac{\partial^{2}V}{\partial x^{2}}(t,x) + rx \partial \frac{\partial V}{\partial x}(t,x)-rV(t,x)=0$$ and the boundary condition $V(T,x)=g(x)$.

There is a replicating portfolio consisting of $x(t)$ shares of $S(t)$ and $y(t)$ units of risk-free assets $A(t)=A(0)e^{rt}$, namely $D(t)=x(t)S(t)+y(t)A(t)$, where $$\begin{cases}
x(t)= \frac{\partial V}{\partial x}(t,S(t))=V_{x}(t, S(t)) \\
y(t)= \frac{V(t,S(t))-V_{x}(t,S(t))S(t)}{A(t)}
\end{cases}$$
