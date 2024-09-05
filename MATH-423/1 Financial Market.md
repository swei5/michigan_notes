[[2024-08-27]] #Valuation #Portfolio #Risks 

### Basic Assumptions
Financial assets described in two categories:
1. Risk-free assets: **guaranteed future value**
2. Risky assets

At this stage, let's consider only two time instants: $t=0, t=1$. Assume we hold a portfolio of stocks $S(t)$ with a position of $x$ and bonds $A(t)$ with a position of $y$ at time $t$.

Here, we assume that:
1. **Randomness**: $S(1)$ is a random variable with **AT LEAST two** different values, $A(1)$ is a **known number**.
2. **Positivity**: $S (t)$, $A(t)>0$ for $t=0,1$.

```ad-important
**Definition 1.1**: Portfolio

The pair $(x,y)$ is a portfolio. $V(t)$ is the value of this portfolio, i.e. the wealth of the investor at time $t$.

$$V(t)=xS(t)+yA(t)$$

We usually assume that $$V(t) \ge 0$$

should not be negative for $t=0,1$.
```

```ad-important
**Definition 1.2**: Admissible

A portfolio is called **admissible** if its value is always **non-negative**.
```

We need to place some further restrictions on $x,y$:
1. **Divisibility** - allowing to purchase fractions of assets 
2. **Short selling** - $x,y$ may be negative
	- We assume a short position **MUST be unwinded**
3. **Liquidity** - both $x,y$ must vary in a **finite range**
	- $\exists M >0$ such that $-M < x, y < M$
	- A market is called liquid if $x,y$ can be arbitrarily large

```ad-important
**Definition 1.3**: Arbitrage 

Arbitrage (“free lunch”) is a **risk-free profit** is made with **NO** **initial investment**.
```

In reality, **arbitrage opportunity** (AO) will **disappear** shortly after its birth, driven by profit-seeking supply and demand forces.

```ad-important
**Definition 1.4**: No-Arbitrage

For any admissible portfolio, $V(0)=0 \implies V(1)=0$ with a **probability of 1**.

In other words, one cannot have $V(1)>0$ with a non-zero probability.
```

---
### One-step Binomial Model
Suppose that $S(0) = 100$, one year later the stock price $S (1)$ either rises up to 120 with probability $p$ or drops down to 90 with probability $1 − p$, where $0<p<1$. The bond prices are $A (0)=100$ and $A (1)=110$.

![[Pasted image 20240827211545.png|400]]


Under this framework, if $S(0)=A(0)$, then $S^{d} < A (1)< S^{u}$, otherwise an **arbitrage opportunity would arise**.

```ad-info
**Proof**

First consider the case $A(1) > S_u$. In other words, we prefer to hold at time $1$ the bond. Then:

![[Pasted image 20240827212125.png|400]]

In other words, we have a **certain strictly positive profit** with **zero** investment ($V(0)=0$). Next, consider case $A(1)$ < S^d.
```

^73727a

In addition, under the One-step Binomial model, we also have that $\frac{S^{d}}{S (0)} < \frac{A (1)}{A (0)} < \frac{S^{u}}{S(0)}$.

### Risk and Return
The **rate of return** is defined by $K_{V}=\frac{V(1)-V(0)}{V(0)}$, the **expected return** is $\mathbb{E}(K_{V})=pK_{V}^{u}+pK_{V}^{d}$ 

The **risk** of this investment is defined as the **standard deviation** of $K_{V}$, $\sigma_{V}=\sqrt{\mathbb{E}(K_{V}-\mathbb{E}(K_{V}))}$.