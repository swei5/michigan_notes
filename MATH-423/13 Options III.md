[[2024-10-07]] #Valuation #Derivatives 

### American Option Valuation
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

If the underlying stock will pay discrete dividends with present value $\text{Div}_{0}$, then the put-call parity estimates are $$$S(0)-\text{Div}_{0}-X\le C_{A}-P_{A} \le S(0)-Xe^{-rT}$$

If dividend yield of the underlying stock is $r_{\text{div}}$, then the put-call parity estimates are $$S(0)e^{-r_{\text{div}}T}-X\le C_{A}-P_{A} \le S(0)-Xe^{-rT}$$
```
