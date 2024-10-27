[[2024-10-24]] #Derivatives #Valuation 

### Martingale Property

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

Further, since $A(n)=A(0)(1+r)^{n}$, the identity may be rewritten as $$\mathbb{E}^{\star}\left[\frac{S(n+1)}{A(n+1)}|S(n)\right]=\frac{S(n)}{A(n)}$$