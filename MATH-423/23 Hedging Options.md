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
Consider a European Call option $C_{E}$ with strike price $X$ and exercise date $T$. Suppose the continuously compounding interest rate is $r$.

If we have $C_E(0)$ at time $0$ and we expect the stock price to go up,  we may
1. We buy the call, then the value at time $T$ is $(S(T)-X)^{+}$
	- We will end up with nothing, however, if $S (T) \le X$
2. Or, we buy $\frac{C_{E}(0)}{S(0)}$ shares of stocks, then the value at $T$ is $\frac{C_{E}(0)}{S (0)} S(T)$
	- We will **always** end up with a positive wealth

If we have $-C_E(0)$ at time $0$ and we expect the stock price to go up,  we may
1. Write and sell a call, then the value at time $T$ is $-(S(T)-X)^{+}$
	- We will end up with $X-S(T)$ if $S(T)>X$
2. Or, short $\frac{C_{E}(0)}{S(0)}$ shares of stocks, then the value at $T$ is $-\frac{C_{E}(0)}{S (0)} S(T)$
	- Recall that $C_{E}(0)<S(0)$; so $\frac{C_{E}(0)}{S (0)} < 1$ and we end up with more value as long as $$X-S(T)< - \frac{C_{E}(0)}{S(0)}S(T)$$

#### Delta Hedging
We view the value $V$ of the portfolio as a function of the underlying stock price $S$, namely $S \to V(S)$. If the stock price changes by a small amount $\Delta S$ and the other parts remain fixed ($r, \sigma$), the change of the **portfolio value** $V$ can be approximated using the Taylor's expansion: $$V(S+\Delta S)-V(S)\approx \frac{\partial}{\partial S}V(S) \Delta S$$
Note that if the derivative $\frac{\partial}{\partial S}V(S)=0$, then $V (S+\Delta S) \approx V(S)$ when $\Delta S$ is **SMALL**, which means that the value $V(S)$ is **insensitive** to a **SMALL CHANGE** in today's price $S$.

```ad-important
**Definition 23.1**: Delta

TODO
```

If we wish to construct a delta-neutral portfolio $V$ that takes position $(x,y,z)$ in stock $S$, risk-free, and the option $D$, then we should have $\Delta_{V}=0$, where $$V(S)=xS+y+zD(S)$$
This means $$0=\Delta_{V} =\frac{\partial}{\partial S}(xS+y+zD(S))=x+z\frac{\partial}{\partial S}D(S)$$So $$x=-z\Delta_{D}$$
Note that there is no restriction on $y$, which can take any value. We usually choose $y$ such that $V(S)=0$.

If the model of stock prices is specified (e.g. BSM), then we could compute $\Delta_{D}$, and construct the delta-neutral portfolio. The process of constructing a delta-neutral portfolio is called **Delta Hedging**.

```ad-example
**Example**: Delta under BSM

Under the BS model, prove the following for a European call/put option at $t=0$, $$\Delta_{C_{E}}=\Phi(d_{+}) \in [0,1]$$ and $$\Delta_{P_{E}}=-\Phi(-d_{+}) \in [-1,0]$$

Recall that $$C_{E}=S \Phi(d_{+}) -Xe^{-rT} \Phi(d_{-})$$ and $d_{\pm}$ depends on $S$: $$d_{\pm}= \frac{\ln \left(\frac{S}{X}\right)-(r\pm \frac{1}{2}\sigma^{2})T}{\sigma \sqrt{T}}$$

This is seemingly simple as if $d_{\pm}$ does not depend on $S$! The proof is done in homework.

We will now verify $\Delta_{P_{E}}=-\Phi(-d_{+})$. From the Put-Call Parity, we have $$C_{E}-P_{E}=S-Xe^{-rT}$$ Therefore, $$\Delta_{P_{E}=} \frac{\partial}{\partial S}(C_{E}-S+Xe^{-rT})= \Delta_{C_{E}}-1=\Phi(d_{+})-1 = -\Phi(-d_{+})$$
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
TODO

---
### Greeks
The last computations dictate that when we expect big changes on the price of the underlying asset in a short time period, we need a better approximation.

We can take into account a second order approximation: $$\frac{\partial^{2}V}{\partial S^{2}}=\frac{\partial \Delta_{V}}{\partial S}$$
Similar to the delta hedging against **small changes** in the stock price, we can hedge against small changes in other variables in the formula $t, r$, and $\sigma$.

This motivates us to study the following Greeks of an asset/portfolio: