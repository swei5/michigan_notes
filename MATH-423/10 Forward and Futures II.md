[[2024-09-26]] #Valuation #Derivatives 

### Forward Contracts Value
Suppose one takes a long position of a forward contract made today on an underlying asset $S$ with delivery date $T$ and forward price $F(0,T)$.

Let's first build on some intuitions:
- Its value today $V(0)=0$ since we can enter a forward contract freely today
- Its value at $T$: $V(T)=S(T)-F(0,T)$
- $V(t)$ should depend on the stock price $S(t)$ and future dividends, probably through $F(t,T)$

```ad-important
**Definition 10.1**: Forward Contracts Values

For any $t\in[0,T]$, the time $t$ value $V(t)$ for a **LONG** forward contractwith forward price $F(0,T)$ is given by $$V(t)=[F(t,T)-F(0,T)]e^{-r(T-t)}$$ where, as we mentioned before, $F (t, T )$ is the forward price of another forward contract made at time $t$.

The value for a **SHORT** position is given by $$V(t)=[F(0,T)-F(t,T)]e^{-r(T-t)}$$
```

```ad-warning
Note the distinction between **Forward Contracts Value** and **[[9 Forward and Futures I#^0734c3|Forward Price]]**.
```

Here, we are **NOT assuming** no dividends; if a stock pays **NO dividends**, then we have $$\begin{align}
V(t)=[F(t,T)-F(0,T)]e^{-r(T-t)}&=[S(t)e^{r(T-t)}-S(0)e^{rT}]e^{-r(T-t)}\\
&= S(t)-S(0)e^{rt}
\end{align}$$
```ad-note
The above implies that if the stock price $S(t)$ grows at the **same rate as a risk-free investment** $S (0) \to S(0)e^{rt}$, then the value of the forward contract will be 0.

Growth of $S(t)$ **ABOVE** the risk free rate $r$ results in a gain for the holder of a long forward position. Intuitively, they would be more likely to buy at time $T$ at the forward price $F(0,T)$ for a stock with high price.
```

```ad-note
**Proof of Definition 10.1**

Proof by no-arbitrage principle: Suppose that we start from $0$ dollar at time $0$. Then, at time $t$:
- **LONG** forward contract with forward price $F(0,T)$: $-V(t)$
- Enter a **SHORT** forward position with forward price $F(t,T)$
- **BORROW** $V(t)$ from the bank: $+V(t)$
- Total Payoff: $0$

At time $T$:
- From **LONG** forward: $S(T)-F(0,T)$
- From **SHORT** forward: $F(t,T)-S(T)$
- From the bank loan: $-V(t)e^{r(T-t)}$

Our total wealth is therefore $$[S(T)-F(0,T)]+[F(t,T)-S(T)]-V(t)e^{r(T-t)}=F(t,T)-F(0,T)-V(t)e^{r(T-t)}$$

Thus, $$\begin{cases}
V(t) < [F(t,T)-F(0,T)]e^{-r(T-t)} & \text{ above gives A.O.} \\
V(t) > [F(t,T)-F(0,T)]e^{-r(T-t)} & \text{ above gives A.O.}
\end{cases}$$

Then, we must have $V(t)=[F(t,T)-F(0,T)]e^{r(T-t)}$.
```

```ad-example
**Example**: Forward Contract with Negative Value 

![[Pasted image 20241006000652.png|500]]
```

---
### Future Contracts
One of the two parties to a forward contract will **LOST** (perhaps a lot of) money at the delivery date $T$. There is always a **risk of default** by the party suffering a loss. **Futures contracts** are designed to eliminate such risk, by involving a stream of (random) payments during the lifetime of the contract.

Like a forward contract, a **futures contract** is based on an **underlying asset** $S(t)$ and has a specific time $T$ of delivery.