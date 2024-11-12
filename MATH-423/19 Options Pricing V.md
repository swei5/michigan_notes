[[2024-11-07]] #Valuation #Derivatives 

### Discussions on Dividends
A company makes a declaration on the **declaration date**. In the declaration, it sets a record date when you must be on the company’s books as a shareholder, in order to receive the dividend. Once the company sets the record date, the **ex-dividend date** is set based on stock exchange rules. The ex-dividend date for stocks is usually set **ONE business day before** the record date.

If you purchase **before the ex-dividend date**, you get the dividend. The dividend will be paid on a later **payable date**.
- If the dividend is 25% or more of the stock value, special rules apply to the determination of the ex-dividend date 

```ad-note
On average, a stock can be expected to **drop by a little less than the dividend amount**. Because stock prices move on a daily basis, the fluctuation caused by small dividends may be difficult to detect.
```

---
### One-step Binomial Model with Dividends
We assume the stock **trades ex-dividend** at time $n$. Then, the **ex-dividend** stock price $S(n)$ is $$S(n)=S(n-1)(1+K(n))-\text{Div}(n)$$
We assume when a holder of a European/American claim exercises on a record date, they **exercise on the ex-dividend stock price**.

Let us defined the **cum-dividend price**, denoted by $\bar{S}$ where $\bar{S}^{u}=S(0)(1+u)$ and $\bar{S}^{d}=S(0)(1+d)$ and the **ex-dividend price** is $S$.

Since the **ex-dividend price is lower** than the cum-dividend price, we do not have the [[15 Options Pricing I#^181293|martingale]] property for the discounted stock price any more: $$S(0) \ne \mathbb{E}^{\star}\left[\frac{S(1)}{1+r}|S(0)\right]$$ since $S(1)$ is an ex-dividend price.

But, note that we still have $$S(0) = \mathbb{E}^{\star}\left[\frac{\bar{S}(1)}{1+r}|S(0)\right]=\mathbb{E}^{\star}\left[\frac{S(1)+\text{Div}}{1+r}|S(0)\right]$$ The discounted wealth is still a martingale. Here, $p^{\star}$ is calculated using the **ex-dividend price** $S(0)$ at time $0$ and the **cum-dividend price** $\bar{S}(1)$ at time $1$.

Now consider a European option $D$ on this stock with payoff function $f$. Its value at time $1$ is given by $D^{u}(1)=f(S^{u}(1))$ and $D^{d}(1)=f(S^{d}(1))$. Note that these are **ex-dividend prices**. We could still use the [[16 Options Pricing II#^4f6bd8|replicating lemma]] to find its value at time $0$:

We construct a replicating portfolio $V=(x,y)$ consisting of $x$ shares of stock $S$ and $y$ units of risk-free asset $A$. Assume without loss of generality that $A(0)=1$, so that $A(1)=1+r$. The pair $(x,y)$ is chosen such that $D(1)=V(1)$. Then, $$\begin{align}
D(0)&=V(0)=xS(0)+y=x\mathbb{E}^\star\left[\frac{\bar{S}(1)}{1+r}|S(0)\right]+y\\
&= x\mathbb{E}^\star\left[\frac{S(1)+\text{Div}}{1+r}|S(0)\right]+y\\
&= \mathbb{E}^\star\left[\frac{x(S(1)+\text{Div})+y(1+r)}{1+r}|S(0)\right]\\
&= \mathbb{E}^\star\left[\frac{V(1)}{1+r}|S(0)\right]\\
&= \mathbb{E}^\star\left[\frac{D(1)}{1+r}|S(0)\right]
\end{align}$$
The discounted option value is still a **martingale**!

---
### $N$ -step Binomial Model with Dividends
We could extend the calculation from the one-step binomial model to $N$ -step general stock model.
- We still first calculate its value at the expiry date $D(N)=f(S(N))$
- Then we go back to calculate $D(n,j)$ at each node $(n,j)$ using the **risk-neutral probability** $p^{\star}$
	- Again, $p^\star_{(n,j)}$ is calculated based on the ex-dividend price $S(n,j)$ and the cum-dividend price $S(n+1)$
	- This is the **ONLY DIFFERENCE**

For American options, we have to further compare the value of waiting $w(n,j)$ and the value of immediate exercising $f(S(n,j))$ that **depends on the ex-dividend price by assumption**.

For European options, the [[17 Options Pricing III#^b11d79|CRR]] is still valid.

```ad-example
**Example**: $N$-step  stock model with Dividends

![[Pasted image 20241112125948.png|500]]

![[Pasted image 20241112130229.png|500]]
```

```ad-note
The premium for the European call **decreases** when the underlying asset **pays dividends**.

The premium for the European put **increases** when the underlying asset **pays dividends**.
```
