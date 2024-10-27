[[2024-10-24]] #Derivatives #Valuation 

### Risk Neutral Probability, Martingale

```ad-question

![[Pasted image 20241025211116.png|400]]

In this example, after one time step, once it becomes known whether the stock price has gone up or down, what would be $\mathbb{E}^{\star}[S(2)]$?
```

Suppose $S(1)=120$. The tree will reduce (i.e. we can erase the lower branch) to the upper branch, which is itself a one-step tree. There are **two possible** values for $S(2)$. In this case, $$\mathbb{E}^{\star}[S(2)|S(1)=120]=120(1+r)$$
Note that this differs from the result seen in the example above because we're standing at $t=1$. Similarly, suppose $S(1)=80$. Then, $$\mathbb{E}^{\star}[S(2)|S(1)=80]=80(1+r)$$ Combining these two cases, we see that $$\mathbb{E}^{\star}[S(2)|S(1)]=S(1)(1+r)$$
We can generalize the above to any time step in the binomial tree model. 

```ad-important
**Definition 15.1**: Risk-neutral Conditional Expectation of $S$

Given that the stock price $S(n)$ has become **known** at time $n$, the risk-neutral conditional expectation of $S(n + 1)$ will be $$\mathbb{E}^{\star}[S(n+1)|S(n)]=S(n)(1+r)$$
```

```ad-note
**Proof of Definition 15.1**

![[Pasted image 20241026165533.png|400]]
```

To simplify the above expression, we define the **discounted stock prices** $$\tilde{S}(n)=S(n)(1+r)^{-n}$$ Then, $$\begin{align}
\mathbb{E}^{\star}[\tilde{S}(n+1)|S(n)]&=(1+r)^{-(n+1)}\mathbb{E}^{\star}[S(n+1)|S(n)]\\
&= (1+r)^{-n}S(n)\\
&= \tilde{S}(n)
\end{align}$$ We say that the discounted stock prices $\tilde{S}(n)$ form a **martingale** under the risk-neutral probability measure $\mathbb{P}^{\star}$. Therefore the probability measure $\mathbb{P}^{\star}$ is also called the **martingale measure**, the probability $p^{\star}$ is also called the **martingale probability**, and the expectation $\mathbb{E}^{\star}$ is also called the **martingale expectation**.

```ad-important
**Definition 15.2**: Martingale

A sequence of random variables $X(0), X(1), X(2), \cdots$ such that $$\mathbb{E}^{\star}[X(n+1)|S(n)]=X(n)$$ for each $n=0,1,2,\cdots$ is said to be a **martingale** with respect to $\mathbb{P}^{\star}$.

This is a **special version** of the definition of martingales for our course purpose.
```

The identity $\mathbb{E}^{\star}[\tilde{S}(n+1)|S (n)]=\tilde{S}(n)$ indeed says that the discounted stock prices $\tilde{S}(0), \tilde{S}(1), \tilde{S}(2), \cdots$ form a martingale under the risk-neutral probability measure $\mathbb{P}^{\star}$.

Further, since $A(n)=A(0)(1+r)^{n}$, the identity may be rewritten as $$\mathbb{E}^{\star}\left[\frac{S(n+1)}{A(n+1)}|S(n)\right]=\frac{S(n)}{A(n)}$$ The result could be extended to **multiple stocks**, **finite time steps**, and **finite possible stock prices**.

```ad-important
**Definition 15.3**: Fundamental Theorem of Asset Pricing

Suppose there are $k$ stocks $S_{1},\cdots, S_{k}$, and a risk-free asset $A$ in the market. Let $S(n)=(S_{1}(n),\cdots, S_{k}(n))$ be the stock price vector and $A(n)$ be the risk-free asset price at time $n$. Suppose $\Omega$ is the set of all scenarios and $\mathbb{P}(\omega)>0$ for each scenario $\omega \in \Omega$.

Then the **No-Arbitrage Principle** is equivalent to the existence of a **risk-neutral probability measure** $\mathbb{P}^{\star}$ on the set of scenarios $\Omega$ such that
- $\mathbb{P}^{\star}(\omega)>0$ for each scenario $\omega \in \Omega$
- The discounted stock prices $\tilde{S}_{i}(n)=S_{i}(n)/A(n)$ are **martingales** for $i=1,\cdots, k$: $$\mathbb{E}^{\star}[\tilde{S}_{i}(n+1)|S (n)]=\tilde{S}_{i}(n)$$

Here the LHS is the **conditional expectation** with respect to the risk-neutral probability $\mathbb{P}^{\star}$ computed once the stock price vector $S(n)$ becomes known at time $t$.
```

```ad-example
**Example**: Computing the risk-neutral probability

![[Pasted image 20241026205948.png|500]]

![[Pasted image 20241026210054.png|500]]

![[Pasted image 20241026210106.png|500]]
```
