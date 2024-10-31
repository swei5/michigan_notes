[[2024-10-07]] #Valuation #Derivatives 

Recall the definition of an **American Option**: ![[11 Options I#^a0fa47]]

```ad-important
**Definition 13.1**: American Options and European Options

For options written on the same underlying asset, with the same strike price $X$ and expiry time $T$ $$C_{A} \ge C_{E} (\ge 0), P_{A} \ge P_{E} (\ge 0)$$
```

^51b66c

```ad-note
**Proof of Definition 13.1**

Intuition: American options provide **at least the same rights** as the European. Let us demonstrate this through no-arbitrage principle. Suppose $C_{A} < C_{E}$

- At time $0$:
	- Buy $1$ American Call: $-C_{A}$
	- Sell $1$ European Call: $+C_{E}$
	- Deposit the remaining: $-(C_{E}-C_{A})$
	- Total payoff: $0$
- At time $T$, assume we hold (**DO NOT exercise**) the American call
	- Total payoff: $(S(T)-X)^{+}-(S(T)-X)^{+}+(C_{E}-C_{A})e^{rT} > 0$

We can use a similar approach to show $P_{A} \ge P_{E}$.
```

^a6b6ea

The value of an American Option is at least the same as the value of the European Option at time $T$. There is a special case for American call option, though.

```ad-important
**Definition 13.2**: American Options without dividends

If the underlying asset pays **NO dividends**, then the European and American calls, written on this asset with the same $X$ and $T$ , have the same price: $$C_{E}=C_{A}$$
```

^ca55d2

```ad-note
**Proof of Definition 13.2**

We have in [[#^51b66c|Definition 13.1]] that $C_{A} \ge C_{E}$. We only need to prove that $C_{A} > C_{E}$ leads to an arbitrage. 

Suppose $C_{A} > C_{E}$ and consider the following portfolio. 
- At time $0$:
	- Sell $1$ American Call: $+C_{A}$
	- Buy $1$ European Call: $-C_{E}$
	- Deposit the remaining: $-(C_{A}-C_{E})$
	- Total payoff: $0$

To proceed, we will discuss in two cases.

**Case 1**: If the American call is exercised at some time $t \le T$ by the other party (the person we sell this American call to):

- Then at time $t$,
	- The other party exercises the American call to buy $1$ share from us
	- We borrow such a share and sell it to them at $X$
	- Deposit $X$

At time $T$,
- European call: $(S(T)-X)^{+}$
- Unwind short position: $-S(T)$
- Initial deposit at time $0$: $(C_{A}-C_{E})e^{rT}$
- Deposit at time $t$: $Xe^{r(T-t)}$
- Total payoff: $$\begin{align}
&(S(T)-X)^{+}-S(T)+(C_{A}-C_{E})e^{rT}+Xe^{r(T-t)} \\  &\ge -X +(C_{A}-C_{E})e^{rT}+Xe^{r(T-t)}>0
\end{align}$$


Case 2: If the American call is **NOT exercised** at all, the total payoff is the same as noted [[#^a6b6ea|here]]: $$(S(T)-X)^{+}-(S(T)-X)^{+}+(C_{E}-C_{A})e^{rT} > 0$$

Combining these two cases. we have A.O. Hence, $C_{A} = C_{E}$.
```

```ad-note
There is no corresponding result like $P_{E}=P_{A}$.
```

If the underlying asset pays no dividends, then we **should NOT exercise** the American call option before the expiry date.

If the underlying asset pays no dividends, the same argument could be used to prove that the values/premia of European and American calls at time $t$ are the same: $C_{E}(t)=C_{A}(t)$

```ad-important
**Definition 13.3**: Put-Call Parity Estimates for American Options

The prices of American put and call options, written on the same underlying asset $S$ that pays **NO dividends**, with the same strike price $X$ and expiry time $T$ satisfy $$S(0)-X\le C_{A}-P_{A} \le S(0)-Xe^{-rT}$$

If the underlying stock will pay discrete dividends with present value $\text{Div}_{0}$, then the put-call parity estimates are $$S(0)-\text{Div}_{0}-X\le C_{A}-P_{A} \le S(0)-Xe^{-rT}$$

If dividend yield of the underlying stock is $r_{\text{div}}$, then the put-call parity estimates are $$S(0)e^{-r_{\text{div}}T}-X\le C_{A}-P_{A} \le S(0)-Xe^{-rT}$$
```

^f99aaa

```ad-note
**Proof of Definition 13.3**

Suppose that the first inequality is not true, i.e. $C_{A}-P_{A}<S(0)-X$. At time $t=0$:
- Long an American Call: $-C_{A}$
- Short an American Put: $+P_{A}$
- Short one share: $+S(0)$
- Deposit the proceeds at risk-free rate $r$: $-(S(0)+P_{A}-C_{A})$
	- Note that $(S(0)+P_{A}-C_{A})>X\ge 0$

Now we discuss two cases:
1. If the American put is exercised at time $t<T$ by the other party
	- Then, our wealth is $(S(t)-X)^{+}+(S(0)+P_{A}-C_{A})e^{rt}-X>Xe^{rt}-X \ge 0$
2. If the American put is not exercised at all
	- We exercise our call option to buy one share for $X$ at time $T$, return it
- At time $T$ our wealth is $(S(0)+P_{A}-C_{A})e^{rT}-X>Xe^{rt}-X \ge 0$

Now, suppose that the second inequality is not true, i.e., $C_{A}-P_{A}<S(0)-Xe^{-rT}$. At time $t=0$:
- Long an American Put: $-P_{A}$
- Short an American Call: $+C_{A}$
- Long one share: $-S(0)$
- Invest (borrow if negative) the difference: $-(C_{A}-P_{A}-S(0))$
	- Note that $(C_{A}-P_{A}-S(0))>-Xe^{-rT}$

Now we discuss two cases:
1. If the American call is exercised at time $t<T$ by the other party
	- Then, our wealth is $(X-S(t))^{+}+X+(C_{A}-P_{A}-S(0))e^{rt}>X-Xe^{-rT}e^{rt} \ge 0$
2. If the American call is not exercised at all
	- We exercise our put option to sell one share for $X$ at time $T$
- At time $T$ our wealth is $X+(C_{A}-P_{A}-S(0))e^{rT}>X-Xe^{-rT}e^{rT}=0$
```

### Bounds on American Option Prices

```ad-important
**Definition 13.4**: Bounds on American Option Prices

The prices of American call and put options, on a stock paying **NO dividends**, satisfy the inequalities

$$\begin{align}
\text{Call:     } & \max{(S(0)-Xe^{-rT},0)} \le C_{A}<S(0)\\
\text{Put:     } & \max{(X-S(0),0)} \le P_{A}<X\\
\end{align}$$
```

The prices of American call and put options, on a stock paying discrete dividends with present value $\text{Div}_{0}$, satisfy the inequalities $$\begin{align}
\text{Call:     } & \max(0,S(0)-X,(S(0)-\text{Div}_{0})-Xe^{-rT}) \le C_{A}<S(0)\\
\text{Put:     } & \max(0,X-S(0),Xe^{-rT}-(S(0)-\text{Div}_{0})) \le P_{A}<X\\
\end{align}$$
---
### American Option Valuation
In general, American options have similar properties to their European counterparts. Difficulties:
- The absence of put-call parity; we only have the **[[#^f99aaa|weaker put-call parity]]** estimates
- The possibility of early exercise

```ad-important
**Definition 13.5**: Option Prices: Dependence on $S$, $X$

The functions $\mathbb{R}_{+} \ni X \to C_{A}(X) \in \mathbb{R}_{+}$ is decreasing, convex, and **[[12 Options II#^09e2e3|Lipschitz with associated constant]]** $1$.

The functions $\mathbb{R}_{+} \ni X \to P_{A}(X) \in \mathbb{R}_{+}$ is increasing, convex, and **Lipschitz with associated constant** $1$.

The functions $\mathbb{R}_{+} \ni S \to C_{A}(S) \in \mathbb{R}_{+}$ is increasing, convex, and **Lipschitz with associated constant** $1$.

The functions $\mathbb{R}_{+} \ni S \to P_{A}(S) \in \mathbb{R}_{+}$ is decreasing, convex, and **Lipschitz with associated constant** $1$.
```

```ad-important
**Definition 13.6**: Option Prices: Dependence on $T$

If $T_{1}<T_{2}$, then $$C_{A}(T_{1})\le C_{A}(T_{2})$$ $$P_{A}(T_{1})\le P_{A}(T_{2})$$ i.e. $T \to C_{A}(T)$ and $T \to P_{A}(T)$ are increasing. The one with longer life is worth more.
```

```ad-note
**Proof of Definition 13.6**

Suppose that $C_{A}(T_{1})>C_{A}(T_{2})$. At time $t=0$,
- Sell one American call expiring at time $T_{1}$: $+C_{A}(T_{1})$
- Buy one American call with same strike price but expiring at time $T_{2}$: $-C_{A}(T_{2})$
- Deposit the difference: $-(C_{A}(T_{1})-C_{A}(T_{2}))$

Then, if the other party exercises the call, we exercise the call as well at the same time $t$ and get positive profit from $(C_{A}(T_{1})-C_{A}(T_{2})$

Similarly, we would not exercise if the other party does not exercise and get positive profit from $(C_{A}(T_{1})-C_{A}(T_{2})$.

We pay prove $P_{A}(T_{1})\le P_{A}(T_2)$ in a similar fashion.
```
