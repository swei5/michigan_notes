[[2024-02-06]] #Portfolio #Investment #Risks

### Efficient Frontier 
Exists in the **mean-standard-deviation** space.

```ad-important
**Definition 6.1**: Efficient Frontier 

The efficient frontier is the set of optimal portfolios that offer the **highest expected return** for a **given level of risk**.

Portfolios on the efficient frontier are **mean-variance efficient** (MVE) in that there is no other combination of stocks that offer that high a return for the risk taken.
```

Portfolios that lie below the efficient frontier are **sub-optimal** because they do not provide enough return for the level of risk.

The optimal risky (**Tangency**) portfolio is where the best CAL meets the efficient frontier of risky assets. It can be obtained by **maximizing** the [[7.2 Risk and Return Profile#^3624b7|Sharpe Ratio]] of the portfolio.
![[Pasted image 20240206133453.png|400]]

---
### Portfolio Selection
Optimization is often carried out in Excel as it's hard to compute by hands.
- When maximizing expected return, we need to set a constant level of constant risk
- When minimizing risk, we need to set a constant level of expected return
	- Inherent risk-reward trade-off

1. Specify the **return characteristics** (i.e., expected returns, variances, covariances) of all assets
2. Find the optimal risky (tangency) portfolio ($T$), obtain the **optimal weight on each risky asset**, calculate the expected return and variance of portfolio $T$  
3. Allocate between the risk-free asset and portfolio $T$, obtain the optimal weight on risk-free and portfolio $T$, calculate the share of the complete portfolio invested in each risky asset
	- Treat portfolio $T$ as one asset and thus $\mathbb{E}(r_T)=r_{f}+\frac{\mathbb{E}(r_{T})-r_{f}}{\sigma_{T}}\sigma_{p}$
---
### Separation Theorem 
All **efficient portfolios** are combinations of **tangency portfolio** and **risk free asset**. The portfolio choice can be separated into two independent tasks
1. Determine tangency portfolio $T$
2. Construct the complete portfolio from T-bill and portfolio $T$

The theorem also states that all investors should hold the risky assets in the **same**  
**proportion**.
- Conservative investors should dilute risky asset holdings with cash, but should **NOT** change composition of those holdings

```ad-summary
**Summary, Markowitz’s Mean-Variance Portfolio Theory**

The optimal portfolio of risky assets is exactly the **same for everyone**, no matter what their tolerance for risk.
1. Investors should control the risk of their portfolio not by reallocating among risky assets, but through the **split between the risk-free and risky assets**
2. The portfolio of risky assets should contain a large number of assets: It should be a **well diversified portfolio**
3. The optimal portfolio of risky assets is the one with the **highest Sharpe ratio**
```

