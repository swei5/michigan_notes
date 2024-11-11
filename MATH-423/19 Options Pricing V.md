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