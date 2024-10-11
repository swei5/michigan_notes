[[2024-10-01]] #Derivatives #Valuation 

```ad-important
**Definition 11.1**: Options

A European call (put) option is a contract giving the holder the right to buy (sell) the underlying asset for a fixed price $X$ at a specified future time $T$.

An American call (put) option is a contract giving the holder the right to buy (sell) the underlying asset for a fixed price $X$ at **any time between now and a future time** $T$.

The price $X$ is called the **strike price** or exercise price.

The time $T$ is called the **exercise time** or expiry time.

Both $X$ and $T$ are set today. The person who buys the option is called the **holder** and the person who sells the option is called the **writer**.
```

^a0fa47

The payoff of a call option at time $T$ is $$\max(S(T)-X,0)$$ also denoted by $(S(T)-X)^+$. The payoff of a put option at time $T$ is $$\max(X-S(T),0)$$ also denoted by $(X-S(T))^{+}$.

![[Pasted image 20241006232204.png|500]]

```ad-note
The payoffs are **non-negative random variables**. This gives the holder’s position an advantage over the position of the writer. So the holder has to **pay** the writer an amount at time $0$ in order to buy the option.
```

The precise value of $C_E$, $C_A$, $P_E$ , $P_A$ depends on the model for the underlying asset. But there are some general relations/bounds.

```ad-important
**Options Assumptions**:
1. Options are freely traded, that is, can readily be bought and sold at the market price
2. Risk-free continuously compounding interest rate $r$
```

```ad-example
**Example**: Payoffs of European options

![[Pasted image 20241006232427.png|500]]

Note that there is A.O. since the expected payoffs for both options do not equal to their premium (payoff should be zero under no arbitrage).

```

### Put-Call Parity
One would expect that there should be some relationship for the put and call premia of options written on the **same underlying asset**, with the **same strike price** and the **same exercise date**.

This relationship, indeed, exists and it is called **put-call parity**.

```ad-important
**Definition 11.2**: Put-Call Parity for European options - No dividends

For an underlying asset paying **no dividends**, the following relationship holds between the prices of European call and put options written on this asset, with the same exercise price $X$ and exercise date $T$: $$C_{E}-P_{E}=S(0)-Xe^{-rT}$$ where $r$ is the **continuously compounding interest rate**.
```

^4f00c5

```ad-note
**Proof of Definition 11.2**

At time 0:
1. Buy a European call option for $C_{E}$: $-C_{E}$
2. Write and sell a European put option for $P_{E}$: $+P_{E}$
3. Short sell one share of the underlying asset for $S(0)$: $+S(0)$
4. Invest $S(0)+P_{E}-C_{E}$ at the interest rate $r$: $-S(0)-P_{E}+C_{E}$
- Total Payoff: $0$

At time $t$:
1. Receive $(S(0)+P_{E}-C_{E})e^{rT}$
2. Two cases: in either way, we pay $X$: $-X$
	- If $S(T) \ge X$, then we exercise the call option and pays $X$ to buy one share; the put is void; the share shorted in (2) is returned: $-X$
	- If $S(T ) \le X$, then the call is void; the holder of the put exercises the right and we have to buy one share for$ X$; the share shorted in (2) is returned: $-X$
- Total Payoff: $(S(0)+P_{E}-C_{E})e^{rT}-X=(S(0)+P_{E}-C_{E}-Xe^{-rT})e^{rT}$
```

```ad-note
$C_{E}$ and $P_{E}$ appear to be depend in the same way on any variables absent in the put-call parity relation $C_{E}-P_{E}=S(0)-Xe^{-rT}$.

Namely, if the **expected return on stock increases**, then intuitively $C_E$ should increase. As a result of the put-call parity relation, $P_E$ should **increase as well**. But intuitively $P_E$ should go down. 

Therefore one would argue that $C_E$ and $P_E$ should be **INDEPENDENT of the expected return on stock** – to be seen from the Black-Scholes formula.
```

We can adapt the put-call parity argument to the case where the **underlying asset pays discrete dividends** with present value $\text{Div}_0$: $$C_{E}-P_{E}=S(0)-\text{Div}_{0}-Xe^{-rT}$$
Or continuously with **dividend yield** $r_{\text{div}}$: $$C_{E}-P_{E}=S(0)e^{-r_{\text{Div}}T}-Xe^{-rT}$$
