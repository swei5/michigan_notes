[[2024-11-26]] #Derivatives #Valuation 

As mentioned in the last lecture, to protect against losses due to **small movements of a parameter**, we could construct a portfolio which is neutral in that parameter, namely the corresponding Greek is zero.

We could also construct a portfolio that protects against losses due to small movements of **multiple parameters**, using multiple European options. We will introduce two examples of this, the **delta-vega** hedging and the **delta-gamma** hedging.

To construct a portfolio that is **delta-vega neutral** or **delta-gamma neutral**, we will consider investments in the stock, the risk-free and **two** European options $D_{1},D_{2}$ based on the underlying stock (with different $X$ and $T$). 
- We need two as the greeks are [[23 Hedging Options I#^93c6e0|related to each other]]

The portfolio position is denoted by $(x,y,z_{1},z_{2})$ and its initial value is $$V=xS+y+z_{1}D_{1}+z_{2}D_{2}$$

---
### Delta-Vega Hedging
If we wish the portfolio to be delta-vega neutral, we should have $$\begin{cases}
0=\nu_{V}= \frac{\partial}{\partial \sigma}(xS+y+z_{1}D_{1}+z_{2}D_{2}) = z_{1}\nu_{D_{1}}+z_{2}\nu_{D_{2}}  \\
0=\Delta_{V}= \frac{\partial}{\partial S}(xS+y+z_{1}D_{1}+z_{2}D_{2}) = x+z_{1}\Delta_{D_{1}}+z_{2}\Delta_{D_{2}}
\end{cases}$$
This is the delta-vega hedging against **small volatility movements** and **small stock price movements**.

If we wish the portfolio to have zero value when it is set up, then $$V=0=xS+y+z_{1}D_{1}+z_{2}D_{2}$$
If the value of any one variable among $(x,y,z_{1},z_{2})$ is known, then we could solve for the other three variables.

```ad-example
**Example**: Delta-Vega Hedging

![[Pasted image 20241203225544.png|500]]

![[Pasted image 20241203225605.png|500]]
```

---
### Delta-Gamma Hedging
Since the delta hedging is not effective for large changes in the stock price, we could renovate it by the so called **delta-gamma hedging** (where the aimed portfolio has zero delta and zero gamma). $$\begin{cases}
\begin{cases}
0=\Gamma_{V}= \frac{\partial}{\partial \sigma}(xS+y+z_{1}D_{1}+z_{2}D_{2}) = z_{1}\Gamma_{D_{1}}+z_{2}\Gamma_{D_{2}}  \\
0=\Delta_{V}= \frac{\partial}{\partial S}(xS+y+z_{1}D_{1}+z_{2}D_{2}) = x+z_{1}\Delta_{D_{1}}+z_{2}\Delta_{D_{2}}
\end{cases}
\end{cases}$$
Similar to **delta-vega hedging**, if any one variable among $(x,y,z_{1},z_{2})$ is known, then we could solve for the other three variables.

```ad-example
**Example**: Delta-Gamma Hedging

![[Pasted image 20241203225901.png|500]]
```

```ad-note
In the above, we only discussed various hedging portfolios over a short time period, e.g. one day. As to long-term hedging, we have to **adjust the hedging portfolio whenever hedged variables change with time**.

In practice it is impossible to hedge in a perfect way by designing a single portfolio to be held for the whole period $[0,T]$.
```

---
### Combination of Options
Options can be used as building blocks to design sophisticated investment instruments. 
- We shall consider an investor with **specific views on the future behavior** of stock prices and willing to take risks
- We will design a portfolio of securities with a prescribed payoff profile that would satisfy this kind of investor

#### Bull Spread
Suppose that the investor expects the stock price to **rise** and wants to gamble on that. 

One simple way is to buy a (European) call option $C^{'}$ with strike price $X^{'}$ that is close to the current stock price. The premium may be reduced by selling a (European) call option $C^{''}$ with strike price $X^{''}>X^{'}$. In this way we can build a so-called bull spread with profit at maturity date $T$:

![[Pasted image 20241203230334.png|400]]

We make a good profit when the stock is **bull** (price is high). It also prevents us from considerable loss when the stock price is low.

#### Bear Spread
Suppose that the investor expects the stock price to **drop** and wants to gamble on that. 

One simple way is to buy a (European) put option $P^{''}$ with strike price $X^{''}$ that is close to the current stock price. The premium may be reduced by selling a (European) put option $P^{'}$ with strike price $X^{'}<X^{''}$. In this way we can build a so-called bull spread with profit at maturity date $T$:

![[Pasted image 20241203231038.png|400]]

We make a good profit when the stock is **bear** (price is low). It also prevents us from considerable loss when the stock price is high.

#### Butterfly
Suppose that the investor believes that the stock price will **stay unaltered** or change insignificantly. A butterfly may be chosen, which is constructed from three call options with strike prices $X^{'} > X^{''} > X^{'''}$: Buy one call with strike $X^{'}$, buy one call with strike $X^{'''}$, and sell two calls with strike $X^{''}$.

![[Pasted image 20241203231242.png|400]]
