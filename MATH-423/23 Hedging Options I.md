[[2024-11-21]] #Derivatives #Valuation

So far we've studied pricing options in **discrete time** models including
- [[17 Options Pricing III|Standard binomial tree]]
- [[18 Options Pricing IV|Independent but not i.d. returns]]
- [[19 Options Pricing V|Path-dependent payoffs]]
- Stock dividends
And in the continuous time model
- [[22 Black-Scholes III|The Black-Scholes model]]

Compared to vanilla stocks,
- Investing in options is more **risky**
- Options can be used to reduce risk in stock trading

To see these, we will analyze the behavior of portfolios consisting of 
- A risky asset $S$
- A risk-free asset $A$ ($A(t)=e^{rt}$ and $A(0)=1$ for simplicity)
- Different types of options written on $S$ - e.g. a call with strike price $X_{1}$ and exercise date $T_{1}$ and a put with strike price $X_{2}$ and exercise date $T_{2}$

We will show how we could use these portfolios for hedging purposes.

---
### Hedging Options
Let's first figure out why investing in options is more risky than investing in stocks. Consider a European Call option $C_{E}$ with strike price $X$ and exercise date $T$. Suppose the continuously compounding interest rate is $r$.

If we have $C_E(0)$ at time $0$ and we expect the stock price to go up,  we may
1. We buy the call, then the value at time $T$ is $(S(T)-X)^{+}$
	- We will end up with nothing, however, if $S (T) \le X$
2. Or, we buy $\frac{C_{E}(0)}{S(0)}$ shares of stocks, then the value at $T$ is $\frac{C_{E}(0)}{S (0)} S(T)$
	- We will **always** end up with a positive wealth

If we have $-C_E(0)$ at time $0$ and we expect the stock price to go up,  we may
1. Write and sell a call, then the value at time $T$ is $-(S(T)-X)^{+}$
	- We will end up with $X-S(T)$ if $S(T)>X$
2. Or, short $\frac{C_{E}(0)}{S(0)}$ shares of stocks, then the value at $T$ is $-\frac{C_{E}(0)}{S (0)} S(T)$
	- Recall that $C_{E}(0)<S(0)$; so $\frac{C_{E}(0)}{S (0)} < 1$ and we end up with more value as long as $$X-S(T)< - \frac{C_{E}(0)}{S(0)}S(T) \iff S(T)> \frac{X}{1- \frac{C_{E}(0)}{S(0)}}>X$$

In either case, we end up losing more investing in options than in stocks in the worse case scenario.

#### Delta Hedging
We view the value $V$ of the portfolio as a function of the underlying stock price $S$, namely $S \to V(S)$. If the stock price changes by a small amount $\Delta S$ and the other parts remain fixed ($r, \sigma$), the change of the **portfolio value** $V$ can be approximated using the Taylor's expansion: $$V(S+\Delta S)-V(S)\approx \frac{\partial}{\partial S}V(S) \Delta S$$
Note that if the derivative $\frac{\partial}{\partial S}V(S)=0$, then $V (S+\Delta S) \approx V(S)$ when $\Delta S$ is **SMALL**, which means that the value $V(S)$ is **insensitive** to a **SMALL CHANGE** in today's price $S$.

```ad-important
**Definition 23.1**: Delta

For an asset (option, forward contract, stock, portfolio) $D$, whose value depends on the price of a risky asset $S$ and is denoted by $D(S)$, the **delta** of the asset $D$ is defined by the derivative $$\Delta_{D}:= \frac{\partial D(S)}{\partial S}$$

A portfolio $V$ is said to be **delta-neutral** if $\Delta_{V}=0$.
```

If we wish to construct a delta-neutral portfolio $V$ that takes position $(x,y,z)$ in stock $S$, risk-free, and the option $D$, then we should have $\Delta_{V}=0$, where $$V(S)=xS+y+zD(S)$$
This means $$0=\Delta_{V} =\frac{\partial}{\partial S}(xS+y+zD(S))=x+z\frac{\partial}{\partial S}D(S)$$So $$x=-z\Delta_{D}$$
Note that there is no restriction on $y$, which can take any value. We usually choose $y$ such that $V(S)=0$.

If the model of stock prices is specified (e.g. BSM), then we could compute $\Delta_{D}$, and construct the delta-neutral portfolio. The process of constructing a delta-neutral portfolio is called **Delta Hedging**.

```ad-example
**Example**: Delta under BSM

Under the BS model, prove the following for a European call/put option at $t=0$, $$\Delta_{C_{E}}=\Phi(d_{+}) \in [0,1]$$ and $$\Delta_{P_{E}}=-\Phi(-d_{+}) \in [-1,0]$$

Recall that $$C_{E}=S \Phi(d_{+}) -Xe^{-rT} \Phi(d_{-})$$ and $d_{\pm}$ depends on $S$: $$d_{\pm}= \frac{\ln \left(\frac{S}{X}\right)-(r\pm \frac{1}{2}\sigma^{2})T}{\sigma \sqrt{T}}$$

We will now verify $\Delta_{P_{E}}=-\Phi(-d_{+})$. From the Put-Call Parity, we have $$C_{E}-P_{E}=S-Xe^{-rT}$$ Therefore, $$\Delta_{P_{E}=} \frac{\partial}{\partial S}(C_{E}-S+Xe^{-rT})= \Delta_{C_{E}}-1=\Phi(d_{+})-1 = -\Phi(-d_{+})$$ by symmetry of the standard normal distribution.
```

```ad-warning
The example above is seemingly simple as if $d_{\pm}$ does not depend on $S$ but in fact it does! The proof is done in homework.
```

Consequently, the delta-neutral portfolio $V$ taking position $(x,y,z)$ with selling a European call option, for example, is $$\begin{cases}
z&= -1 \\
x&=-z\Delta_{C_{E}}=\Phi(d_{+})
\end{cases}$$ and $$\begin{align}
V(S)&=xS+y+zC_{E} \\
&= \Phi(d_{+})S+y-(S \Phi(d_{+}) -Xe^{-rT} \Phi(d_{-}))\\
&= y+e^{-rT}X \Phi(d_{-})
\end{align}$$
If we choose $y$ such that $V(S)=0$, then $$y=-e^{-rT}X \Phi(d_{-})$$
Therefore the delta-neutral portfolio $V$, which is worth $0$ initially and sells a European call option, has the position $$(\Phi(d_{+}),-e^{-rT}X \Phi(d_{-}),-1)$$
Note that the position will definitely change if we wish the portfolio has another number of European call/put options.

```ad-example
**Example**: The Effect of Delta-Hedging

![[Pasted image 20241123163724.png|450]]
![[Pasted image 20241123163748.png|450]]
![[Pasted image 20241123164104.png|450]]

Note that changes in delta-hedged portfolio $V$ is significantly smaller than that in an unhedged portfolio, which is subject to movements in $S$.
```

Although the delta-hedging considerably reduces our loss when $S\left(\frac{1}{365}\right)$ rises up above $100$ but on the other hand, prevents us from enjoying the possible big gain when $S\left(\frac{1}{365}\right)$ drops below 100.

```ad-note
The real purpose of hedging is to protect us from **any tremendous loss** even at the cost of big-gain opportunity. Beware that there is no perfect strategy which reduces the risk and brings profits simultaneously.

However, delta-hedging is **ONLY effective** for small change in stock price (small $S(\frac{1}{365})-S(0)$) in the example above.
```

```ad-example
**Example**: Delta Hedging when stock price changes are considerable

![[Pasted image 20241123173147.png|450]]

If we fear that such large changes might happen, the above hedging is not a satisfactory solution.
```

If we do not hedge, at least we have a gamble with a positive outcome whenever the stock price goes down. While if we use the delta-neutral portfolio, no matter whether the stock price goes up or down, **we may have losses**, though considerably smaller than the naked position if the stock price goes up.

---
### Greeks
The last computations dictate that when we expect big changes on the price of the underlying asset in a short time period, we need a better approximation.

We can take into account a second order approximation: $$\frac{\partial^{2}V}{\partial S^{2}}=\frac{\partial \Delta_{V}}{\partial S}$$
Similar to the delta hedging against **small changes** in the stock price, we can hedge against small changes in other variables in the formula $t, r$, and $\sigma$.

This motivates us to study the following Greeks of an asset/portfolio.

```ad-important
**Definition 23.2**: Greeks

For a general asset/portfolio $D$, denote its value by $D(S(t),t,\sigma, r)$. We can define the associated Greeks to describe the sensitivity of the price $D$ with respect to $S(t)$, $t$, $\sigma$, and $r$.
- $\Delta_{D}$ denotes $\frac{\partial D}{\partial S(t)}$
- $\Gamma_{D}$ denotes $\frac{\partial^{2} D}{\partial S(t)^{2}}$
- $\Theta_{D}$ denotes $\frac{\partial D}{\partial t}$
- $\nu_{D}$ denotes $\frac{\partial D}{\partial \sigma}$ (Vega)
- $\rho_{D}$ denotes $\frac{\partial D}{\partial r}$
```

Note that $S(t)$, $t$, $\sigma$ and $r$ are treated as **independent variables**, so all of these partial derivatives are calculated with other variables remaining the same.

We could calculate Greeks once we have the precise stock model. Then, for small changes $\Delta S$, $\Delta t$, $\Delta \sigma$, $\Delta r$ in the variables, we have the following approximation of the increment $\Delta V$ of the value $V$ of a portfolio, thanks to the Taylor's Expansion: $$\Delta V \approx \Delta_{V}\Delta S+\Theta_{V}\Delta t+\nu_{V} \Delta \sigma + \rho_{V} \Delta r + \frac{1}{2} \Gamma_{V} (\Delta S)^{2}$$
From this we see that, in order to make the value of the portfolio **insensitive to small changes in certain variable**(s), we need to neutralize the corresponding Greek(s) of the portfolio.

For example, to hedge against small volatility movements we should construct a vega-neutral portfolio, with vega of the portfolio equal to zero.

```ad-summary
**Options Greeks for European Calls, closed forms under BSM**
- **Delta**: $\frac{\partial D}{\partial S(t)}=\Delta_{C_{E}}(t)= \Phi(d_{+}(t))$
- **Gamma**: $\frac{\partial^{2} D}{\partial S(t)^{2}}=\Gamma_{C_{E}}(t)= \frac{1}{S(t)\sigma\sqrt{2\pi(T-t)}}\exp(-\frac{1}{2}d_{+}^{2}(t))$
- **Theta**: $\frac{\partial D}{\partial t}=\Theta_{C_{E}}(t)= - \frac{S(t)\sigma}{2 \sqrt{2\pi (T-t)}} \exp\left(- \frac{1}{2} d_{+}^{2}(t)\right)- rXe^{-r(T-t)}\Phi(d_{-}(t))$
- **Vega**: $\frac{\partial D}{\partial \sigma}=\nu_{C_{E}}(t)=\frac{S(t)\sqrt{T-t}}{\sqrt{2\pi}}\exp\left(- \frac{1}{2} d_{+}^{2}(t)\right)$
- **Rho**: $\frac{\partial D}{\partial r}=\rho_{C_{E}}(t)=(T-t)Xe^{-r(T-t)}\Phi(d_{-}(t))$

Greeks are functions of $S(t), t, \sigma, r$, but we write $(t)$ for short.
```

The Greeks of European put can be calculated similarly by using the Black-Scholes formula or the **Put-Call Parity**.

```ad-note
In general, according to the Black-Scholes PDE, the price $D$ of any European derivative security satisfies the following equation $$\frac{\partial D}{\partial t}+rS \frac{\partial D}{\partial S}+ \frac{1}{2} \sigma^{2}S^{2} \frac{\partial ^{2}D}{\partial S^{2}}=rD$$
```
