[[2024-09-24]] #Valuation #Derivatives 

### Forward Contracts

```ad-important
**Definition 9.1**: Forward Contract

A forward contract is an agreement to buy or sell a risky asset (typically a stock) at a fixed future date $T$ , called **delivery date**, for a price $F (0, T )$ determined today, called **forward price**.
```

The principal reason for entering into a forward contract is to become **independent of the unknown future price** of a risky asset. In the agreement, the party obliged to buy (sell) the asset is said to take a long (short) forward position.

Forward contracts are designed to enter freely, i.e. **NO initial payment**. The forward price **IS NOT** the **value** of the forward contract.

```ad-note
The future value of a **LONG** forward position at time T is $S(T )−F (0, T )$.

The future value of a **SHORT** forward position at time T is $F (0, T )−S(T )$.

![[Pasted image 20240926183728.png|500]]
```

```ad-important
**Definition 9.2**: Forward Price

The no-arbitrage forward price is $$F(0,T)=S(0) \frac{A(T)}{A(0)}=S(0)e^{rT}$$

where $r$ is the continuous compounding interest rate.
```

^0734c3

Note that there is **NO** $S(T)$ in the expression of $F(0,T)$ - because $F(0,T)$ is **determined today**.
- Otherwise arbitrage opportunities arise
	- If $F(0,T)<S(0)e^{rT}$, for example
		- At time 0
			- Short $S$: $+S(0)$
			- Long Forwards of $S$ at $F(0,T)$
			- Long bonds using proceeds: $-S(0)$
			- Total payoff: $0$
		- At time $T$
			- Short bonds: $+S(0)e^{rT}$
			- Long $S$ by forwards: $-F(0,T)$
			- Total payoff: $S(0)e^{rT}-F(0,T)$
	- If $F(0,T)=S(T)$
		- We consider (1) $S (T) > S(0)e^{rT}$ and (2) $S(T)<S(0)e^{rT}$ - both of which involve arbitrage opportunities as shown

```ad-note
However, if $F(0,T)<S(0)e^{rT}$ but **short sales of stocks** are **prohibited**, then there is **NO arbitrage opportunities**.
```

Some variations in definition 9.2 may include: $$F(0,T)=S(0)\left(\frac{1+r}{m}\right)^{mT}$$ if interest is periodically compounded with frequency $m$ and nominal interest rate $r$. Or,  $$F(0,T)=S(0) \frac{1}{B(0,T)}$$ in terms of **zero-coupon bond** - with no assumption about constant interest rate.

```ad-note
If contract starts at time $t \in (0,T)$, then $$F(t,T)=S(t) \frac{A(T)}{A(t)}=S(t)e^{r(T-t)}$$
Note that $\lim_{t \to T} F(t,T)=S(t)$ if $S(t)$ is a continuous process.
```

```ad-example
**Example**: Forward Contracts (no dividend)

![[Pasted image 20240926233935.png|500]]

![[Pasted image 20240926234256.png|500]]
```

---
### Forward Contracts with Dividends
We will understand the dividend as a sum of money paid to the holder of an asset.
- The **difference on the interest rates** can be regarded as **continuously paid dividend**
- The term **holding cost** describes the cash flow involved in **maintaining a long or a short position in the underlying asset**

```ad-important
**Definition 9.3**: Forward Price (Dividends)

The forward price of a stock paying dividend $\text{Div}$ at time $t \in (0,T)$ is $$F(0,T)=[S(0)-e^{-rt} \text{Div}]e^{rT}= \frac{S(0)-B(0,T)\text{Div}}{B(0,T)}$$

If $\text{Div}=0$, then we get [[#^0734c3|Definition 9.2]] exactly.

More generally, the formula can easily be generalized to the case when dividends are paid **more than once** within $(0,T)$: $$F(0,T)=[S(0)-\text{Div}_{0}]e^{rT}=S(0)e^{rT}-\text{Div}_{T}$$ where $\text{Div}_{0}$ (resp. $\text{Div}_{T}$) stands for present (resp. future) values of all dividends: $$\text{Div}_{0}=\sum\limits_{i=1}^{n} d_{i}e^{-rt_{i}}$$ $$\text{Div}_{T	}=\sum\limits_{i=1}^{n} d_{i}e^{r(T-t_{i})}$$ for some dividend $d_{i}$ paid at time $t_{i}$.
```

```ad-note
**Proof of Definition 9.3**

Proof by no-arbitrage principle: Suppose that we start from $0$. If $F(0,T) > [S(0)-e^{-rt} \text{Div}]e^{rT}$, then:
- At time $0$
	- Short forward 
	- Long $S$: $-S(0)$
	- Borrow $S(0)$ from bank: $+S(0)$
	- $V(0)=0$
- At time $T$
	- Short $S$ for $F(0,T)$: $+F(0,T)$
	- Obain dividends in cash at time $t$: $+\text{Div}e^{r(T-t)}$
	- Pay back the bank: $-S(0)e^{rT}$
	- $V(T)=F(0,T)-[S(0)-e^{-rt} \text{Div}]e^{rT} > 0$
```

What if the dividend is paid continuously at a rate $r_{div}$, and the dividends are **reinvested** in the stock? I.e., what if the dividend is **paid continuously in the form of extra shares** rather than cash?

```ad-important
**Definition 9.4**: Forward Price (Reinvested Dividends)

For an underlying asset paying continuous dividend with rate $r_{div}$ , if a forward contract is initiated at time $t < T$, then the forward price is $$F(t,T)=S(t)e^{(r-r_{div})(T-t)}=S(t)e^{-r_{div}(T-t)}e^{r(T-t)}$$
```

```ad-example
**Example: Interest Rate Parity**

![[Pasted image 20240927004844.png|500]]
```
