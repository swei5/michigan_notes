[[2024-08-27]] #Valuation #Portfolio

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

TODO
```

We need to place some further restrictions on $x,y$:
1. **Divisibility** - allowing to purchase fractions of assets 
2. **Short selling** - $x,y$ may be negative
	- We assume a short position **MUST be unwinded**
3. **Liquidity** - both $x,y$ must vary in a **finite range**
	- $\exists M >0$ such that $-M < x, y < M$
	- A market is called liquid if $x,y$ can be arbitrarily large

In reality, **arbitrage opportunity** (AO) will **disappear** shortly after its birth, driven by profit-seeking supply and demand forces.

```ad-important
**Definition 1.3**: No-Arbitrage

For any admissible portfolio, $V(0)=0 \implies V(1)=0$ with a **probability of 1**.

In other words, one cannot have $V(1)>0$ with a non-zero probability.
```

---
### One-step Binomial Model
Under this framework, if $S(0)=A(0)$, then $S^{d} < A (1)< S^{u}$, then an **arbitrage opportunity would arise**.

TODO: Another Assumption 

The **rate of return** is defined by $K_{V}=\frac{V(1)-V(0)}{V(0)}$, the **expected return** is $\mathbb{E}(K_{V)}=pK_{V}^{u}+pK_{V}^{d}$ 

The **risk** of this investment is defined as the **standard deviation** of $K_{V}$, $\sigma_{V}$.