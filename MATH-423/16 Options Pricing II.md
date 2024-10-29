[[2024-10-29]] #Valuation #Derivatives 


```ad-important
**Definition 16.1**: European Derivative Securities

An **European derivative security** (or European contingent claim), denoted by $D$ for example, is a contract whose **payoff** at its maturity $T$ is in the form of $D(T)=f(S(T))$, where $f$ is a given function and $S$ denotes the underlying asset. For example, 
- European call option: $f(S)=(S-X)^{+}$
- Forward contract: $f(S)=S-X$
```

Similarly, an American derivative security (or American contingent claim) is a contract which can be exercised at any time $n \le T$ with payoff $f(S(n))$.

### One-step Binomial Tree Model
Consider the One-step Binomial tree Model. The stock price is given by $$\begin{align}
S(0) \text{ and }S(1)=\begin{cases}
S^{u}=S(0)(1+u) & \text{ with probability } p,\\
S^{d}=S(0)(1+d) & \text { with probability } 1-p,
\end{cases}
\end{align}$$ and the risk-free asset is given by $$A(1)=A(0)(1+r)$$
It is assumed that $-1<d<r<u$ and $p\in (0,1)$. If we are interested in calculating $C(0)$ for a call option with strike price $X$, we could use the **Replicating Lemma** to replicate the call option.

```ad-note
**Replicating Lemma**

If asset $M_{1}$ is worth the same as asset $M_{2}$ at time $t=1$, i.e. $M_{1}(1)=M_{2}(1)$, then $M_{1}(0)=M_{2}(0)$, otherwise an arbitrage opportunity would arise.
```

We may show this by constructing a portfolio $V=(x,y)$ with $x$ shares of stock $S$ and $y$ units of risk-free assets $A$, such that it will produce the same payoff as the **call option** at $t=1$. That is, we set $C(1)=V(1)$, where $$C(1)=(S(1)-X)^{+}, V(1)=xS(1)+yA(1)$$ this leads to two linear equations in terms of $x$ and $y$ (since the stock price may go up or down at z $t=1$):  $$\begin{align}
xS^{u}+yA(1) &= (S^{u}-X)^{+} \\
xS^{d}+yA(1) &= (S^{d}-X)^{+}
\end{align}$$ we can hence solve for $(x,y)$. More generally, we can extend this to **all European derivative securities** $D$  with some payoff function $f$.  We can write the above as $$\begin{align}
xS^{u}+yA(1) &= f(S^{u}) \\
xS^{d}+yA(1) &= f(S^{d})
\end{align}$$ and hence solve the equations as  $$\begin{align}
\begin{cases}
x&= \frac{f(S^{u})-f(S^{d})}{S^{u}-S^{d}}\\
y&= \frac{f(S^{d})-xS^{d}}{A(1)}
\end{cases}
\end{align}$$
Then, by no-arbitrage principle and the replicating lemma, we must have $D(0)=V(0)$. Therefore, $$D(0)=xS(0)+yA(0)$$ Note that everything on the RHS is **known**. Also note that $$yA(0)=(f(S^{d})-xS^{d}) \frac{A(0)}{A(1)}=\frac{f(S^{d})-xS^{d}}{1+r}$$
Let us calculate the **discounted option price**: $$\begin{align}
\frac{D(0)}{A(0)}&=x \frac{S(0)}{A(0)}+y\\
&= x\mathbb{E}^{\star}\left[\frac{S(1)}{A(1)}|S(0)\right]+y & \text{ Definition 15.2}\\
&= \mathbb{E}^{\star}\left[\frac{xS(1)+yA(1)}{A(1)}|S(0)\right] & \text{ linear property of expectation}\\
&= \mathbb{E}^{\star}\left[\frac{D(1)}{A(1)}|S(0)\right] & \text{ replicating portfolio }
\end{align}$$ where $p^{\star}$ and $\mathbb{P}^{\star}$ are the risk-neutral probability (measure) of binomial tree model. So we have the following [[15 Options Pricing I#^181293|Martingale Property]]: $$\tilde{D}(0)=\mathbb{E}^{\star}\left[\tilde{D}(1)|S(0)\right]=\mathbb{E}^{\star}\left[\tilde{D}(1)\right]$$ where $\tilde{D}(n)=\frac{D(n)}{A(n)}$, or equivalently, $\tilde{D}(n)=\frac{D(n)}{(1+r)^{n}}$ in the binomial tree model. The **second equality** holds because $S(0)$ is **known**.

```ad-note
The risk-neutral probability $p^{\star}$ is a quick way to **price** the European option with payoff $f$, compared to the method of replicating portfolios.
```

TODO: EXAMPLE
#### American Options
We have to consider the right to exercise early and compare 
- The payoff we obtain by exercising immediately, with
- The present value of the future payoffs

Then, we should act accordingly, namely, choose the one that **offers the highest value**. More specifically:
- At time $1$: We only exercise if we have a **positive payoff**
- At time $0$: We exercise at time $0$ only if that $$f(S(0))\ge \mathbb{E}^{\star}\left[\frac{f(S(1))}{1+r}|S(0)\right]$$ otherwise we wait. Therefore, the price of the American option is given by $$D(0)=\max(f(S(0)),\frac{p^{\star}f(S^{u})+(1-p^{\star})f(S^{d})}{1+r})$$
---
### Two-step Binomial Tree Model
