[[2024-09-17]] #Portfolio #Valuation #Risks 

### Efficient Frontier
Let $V_{i}, i=1,2$ be two risky assets, e.g., stock price, portfolio etc., with associated data $(\sigma_{i},\mu_{i}), i=1,2$. Then,
1. If $\mu_{1}=\mu_2$, we naturally prefer the asset with **lower risk**
2. If $\sigma_{1}= \sigma_2$, we naturally prefer the asset with **higher expected return**
3. If $\sigma_{1} \le \sigma_{2}$ and $\mu_{1} \le \mu_2$, i.e., high risk-high returns vs. low risk-low returns, then depends on the **profile** of the investor

```ad-important
**Definition 7.1**: Dominant Portfolio, Efficient Frontier

We say that stock (security, or portfolio) 1 **dominates** (or is more preferable than) stock (security, or portfolio) if $\mu_{1} \ge \mu_2$ and $\sigma_{1} \le \sigma_2$.

A portfolio is called **efficient** if it is **NOT dominated** by any other portfolio in the attainable set. The set of all efficient portfolios is called the **efficient frontier**.
```

![[Pasted image 20240923144619.png|300]]

Graphically in Markowitz's Bullet, this is represented by the red line in bold. In other words, this is the region where the portfolio has
- Highest expected return among all attainable portfolios with the same risk 
- Lowest risk among all attainable portfolios with the same expected returns

For our convenience, we will denote the weights of the portfolios lying on the efficient frontier by $w_{EF}$ instead of $w_{\mu_{V}}^{\min}$, for $\mu_{V} \ge \mu_{MVP}$.

The efficient frontier is the set of points whose weights $w_{EF}$ are given by $$w_{EF}=A\mu+B$$ for $\mu > \mu_{MVP}$ and $A=\frac{(am-cu)C^{-1}}{ad-bc}$ and $B=\frac{(du-bm)C^{-1}}{ad-bc}$.