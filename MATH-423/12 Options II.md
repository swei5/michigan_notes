[[2024-10-03]] #Derivatives #Valuation 

```ad-important
**Definition 12.1**: Bounds on European Option Prices

The prices of European call and put options on a stock **paying NO dividends** satisfy the inequalities $$\begin{align}
\text{Call:     } & \max{(S(0)-Xe^{-rT},0)} \le C_{E}<S(0)\\
\text{Put:     } & \max{(Xe^{-rT}-S(0),0)} \le P_{E}<Xe^{-rT}\\
\end{align}$$

![[Pasted image 20241011110630.png|400]]
```

In the above inequalities, if discrete dividends will be paid on the stock, we **subtract** the **PV** $\text{Div}_{0}$ of the dividend from $S(0)$: $$\begin{align}
\text{Call:     } & \max{(S(0)-\text{Div}_{0}-Xe^{-rT},0)} \le C_{E}<S(0)-\text{Div}_{0}\\
\text{Put:     } & \max{(Xe^{-rT}-(S(0)-\text{Div}_{0}),0)} \le P_{E}<Xe^{-rT}\\
\end{align}$$
If dividend yield of the stock is $r_{\text{div}}$, then discount $S (0)$ by the factor $e^{-r_\text{div}T}$: $$\begin{align}
\text{Call:     } & \max{(S(0)e^{-r_\text{div}T}-Xe^{-rT},0)} \le C_{E}<S(0)e^{-r_\text{div}T}\\
\text{Put:     } & \max{(Xe^{-rT}-S(0)e^{-r_\text{div}T},0)} \le P_{E}<Xe^{-rT}\\
\end{align}$$
```ad-note
**Proof of Definition 12.1**

From the [[11 Options I#^4f00c5|put-call parity]] we have $$P_{E}=C_{E}-S(0)+Xe^{-rT}$$ Hence, $P_{E}\ge 0$ implies $$C_{E} \ge S(0)-Xe^{-rT} \ge 0 \implies C_{E} \ge \max{(S(0)-Xe^{-rT},0)}$$

Similarly, $C_{E}\ge 0$ implies $$P_{E} \ge Xe^{-rT}-S(0) \ge 0 \implies P_{E} \ge \max{(Xe^{-rT}-S(0),0)}$$

This completes the first half of our proof.

Let's assume that $C_{E} < S(0)$, then $P_{E} < Xe^{-rT}$. So it remains to prove $C_{E} < S(0)$. We will again use the no-arbitrage principle to prove $C_{E} < S(0)$. 

By contradition, we assume that $C_{E} \ge S(0)$.

At time $0$:
- Short Call: $+C_{E}$
- Long one share: $-S(0)$
- Deposit the difference: $C_{E}-S(0)$
- **Total payoff**: $0$

At time $T$:
- Collect deposit: $+(C_{E}-S(0))e^{rT}$
- Sell one share: $+S(T)$
- Close short call position: $-(S(T)-X)^{+}$
	- Pay $0$ if $X > S(T)$
	- Pay the price difference if $S(T) > X$
- **Total payoff**: $S(T)-(S(T)-X)^{+}+(C_{E}-S(0))e^{rT} > (C_{E}-S(0))e^{rT} \ge 0$ since $S(T)-(S(T)-X)^{+} \ge 0$

A.O. So we cannot assume $C_{E} \ge S(0)$. This completes the proof.
```

### Option Valuation
We have seen from the put-call parity that $C_E$ and $P_E$ **do NOT depend on the expected return** of the underlying asset, when other parameters $(X, S, T)$ in the put-call parity are **fixed**.

How about the dependence of $C_E$ and $P_E$ on each of $(X, S, T)$, when the other two parameters are fixed?

```ad-important
**Definition 12.2**: Replicating Lemma, Comparison Lemma

**Replicating Lemma**: If portfolio $V_1$ is worth the same as portfolio $V_2$ at time $T$, i.e., $V_{1}(T)=V_{2}(T)$, then $V_{1}(0)=V_{2}(0)$, otherwise an arbitrage opportunity would arise.

**Comparison Lemma**: If portfolio $V_1$ is worth **NO MORE than** portfolio $V_2$ at time $T$, i.e., $V_{1}(T)\le V_{2}(T)$, then $V_{1}(0)\le V_{2}(0)$, otherwise an arbitrage opportunity would arise.
```

^56f05a

With these, we can propose the following.

```ad-important
**Definition 12.3**: Option Prices: Dependence on $X$

If $X_{1} < X_{2}$, then $$C_{E}(X_{1}) \ge C_{E}(X_{2})$$ i.e. $X \to C_{E}(X)$ is **decreasing**.

If $X_{1} < X_{2}$, then $$P_{E}(X_{1}) \le P_{E}(X_{2})$$ i.e. $X \to P_{E}(X)$ is **increasing**.
```

^924ee7

To prove this, since $X_{1} < X_{2}$, we always have $(S (T)-X_{1})^{+} \ge (S (T)-X_{2})^{+}$. Therefore by [[#^56f05a|Definition 12.2]], $C_{E}(X_{1}) \ge C_{E}(X_{2})$. We can show it for the put option using a similar approach.

```ad-important
**Definition 12.4**: Option Prices: Dependence on $X$, Cont'd

If $X_{1}<X_2$, then $$C_{E}(X_{1})-C_{E}(X_{2})\le e^{-rT}(X_{2}-X_{1})$$ $$P_{E}(X_{2})-P_{E}(X_{1})\le e^{-rT}(X_{2}-X_{1})$$
```

^4c5b41

We can show this again using put-call parity: $$C_{E}(X_{1})-P_{E}(X_{1})=S(0)-X_{1}e^{-rT}$$$$C_{E}(X_{2})-P_{E}(X_{2})=S(0)-X_{2}e^{-rT}$$ Taking the difference and we get $$[C_{E}(X_{1})-C_{E}(X_{2})]+[P_{E}(X_{2})-P_{E}(X_{1})]=[X_{2}-X_{1}]e^{-rT}$$
Since $X_{1}<X_{2}$, from [[#^924ee7|Definition 12.3]] we know that all three differences are **NON-negative**. So, each difference on the left is no more than the RHS. 

```ad-important
**Definition 12.5**: Option Prices: Dependence on $X$, Cont'd

Combining [[#^924ee7|Definition 12.3]] and [[#^4c5b41|Definition 12.4]], we immediately have that $$|C_{E}(X_{1})-C_{E}(X_{2})|\le e^{-rT}|X_{2}-X_{1}|$$ $$|P_{E}(X_{2})-P_{E}(X_{1})|\le e^{-rT}|X_{2}-X_{1}|$$

Here, the functions $X \to C_{E}(X)$ and $X \to P_{E}(X)$ are **[[#^09e2e3|Lipschitz with associated constant]]** $e^{-rT}<1$.
```

```ad-important
**Definition 12.6**: Lipschitz with associated constant

Let $f: \mathbb{R}_{+} \to \mathbb{R}_{+}$. The function $f$ is called **Lipschitz with associated constant** $K$ if for all $x,y \in \mathbb{R}_{+}$, $$|f(x)-f(y)| \le K|x-y|$$
```

^09e2e3

```ad-important
**Definition 12.7**: Convex Combination, Convex Function

Let $x,y$ be elements of a linear space. The element $\alpha x+(1-\alpha)y$ for some $\alpha \in [0,1]$ is **an element of the linear space** and will be called the **convex combination** of $x,y$.

Let $f: \mathbb{R}_{+} \to \mathbb{R}_{+}$. The function $f$ is **convex** if for all $x,y \in \mathbb{R}_{+}$ and $\alpha \in [0,1]$, $$f(\alpha x +(1-\alpha)y) \le \alpha f(x) + (1-\alpha)f(y)$$
```

```ad-important
**Definition 12.8**: Option Prices: Dependence on $X$, Cont'd

The functions $X \to C_{E}(X)$ and $X \to P_{E}(X)$ are **convex**, i.e. for any $X_{1}, X_{2} > 0$ and any $\alpha \in [0,1]$, $$C_{E}(\alpha X_{1}+(1-\alpha)X_{2}) \le \alpha C_{E}(X_{1})+(1-\alpha)C_{E}(X_{2})$$ $$P_{E}(\alpha X_{1}+(1-\alpha)X_{2}) \le \alpha P_{E}(X_{1})+(1-\alpha)P_{E}(X_{2})$$

Let $S \in \mathbb{R}_{+}$ be fixed. The functions $\mathbb{R}_{+}\ni X \to (S-X)^{+} \in \mathbb{R}_{+}$ and $\mathbb{R}_{+}\ni X \to (X-S)^{+} \in \mathbb{R}_{+}$.
```

^547734

```ad-note
**Proof of Definition 12.8**

We will consider only the call option case. For notational convenience, we fix $X_{1},X_{2}>0$ and $\alpha \in [0,1]$ and define $$X^{\alpha}=\alpha X_{1}+(1-\alpha) X_{2}$$

Compare two portfolios: At time $0$,
1. Buy one call with $X^{\alpha}$
2. Buy $\alpha$ calls with $X_{1}$ and $1-\alpha$ calls with $X_2$

At time $T$, each payoff is
1. $(S(T)-X^{\alpha})^{+}$
2. $\alpha(S(T)-X_{1})^{+} + (1-\alpha)(S(T)-X_{2})^{+}$

From [[#^547734|Definition 12.8]] we know that $$(S(T)-X^{\alpha})^{+} \le \alpha(S(T)-X_{1})^{+} + (1-\alpha)(S(T)-X_{2})^{+}$$

And by [[#^56f05a|Definition 12.2]] we have that $$C_{E}(X^{\alpha}) \le \alpha C_{E}(X_{1})+(1-\alpha) C_{E}(X_{2})$$

The proof for the put option case is similar.
```


