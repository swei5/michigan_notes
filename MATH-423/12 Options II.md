[[2024-10-03]] #Derivatives #Valuation 

```ad-important
**Definition 12.1**: Bounds on European Option Prices

The prices of European call and put options on a stock **paying NO dividends** satisfy the inequalities $$\begin{align}
\text{Call:     } & \max{(S(0)-Xe^{-rT},0)} \le C_{E}<S(0)\\
\text{Put:     } & \max{(Xe^{-rT}-S(0),0)} \le P_{E}<Xe^{-rT}\\
\end{align}$$
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

From the put-call parity we have $$P_{E}=C_{E}-S(0)+Xe^{-rT}$$ Hence, $P_{E}\ge 0$ implies $$C_{E} \ge S(0)-Xe^{-rT} \ge 0 \implies C_{E} \ge \max{(S(0)-Xe^{-rT},0)}$$

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
**Definition 12.3**: Option Prices: Dependence on $X$:

If $X_{1} < X_{2}$, then $$C_{E}(X_{1}) \ge C_{E}(X_{2})$$ i.e. $X \to C_{E}(X)$ is **decreasing**.

If $X_{1} < X_{2}$, then $$P_{E}(X_{1}) \le P_{E}(X_{2})$$ i.e. $X \to P_{E}(X)$ is **increasing**.
```

To prove this, since $X_{1} < X_{2}$, we always have $(S (T)-X_{1})^{+} \ge (S (T)-X_{2})^{+}$. Therefore by [[#^56f05a|Definition 12.2]], $C_{E}(X_{1}) \ge C_{E}(X_{2})$. We can show it for the put option using a similar approach.

