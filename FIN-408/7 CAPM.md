[[2024-02-08]] #CAPM #Investment #Risks #Portfolio 

To **operationalize mean-variance analysis** we need estimates of expected returns, variances, and covariances.
- Expected returns are especially hard to estimate 

### CAPM
Changes the concept of the risk of an asset/portfolio from that of standard deviation to that of $\beta$.

![[8.2 CAPM#^b3afc2]]

The market portfolio will be **on the efficient frontier** (for a given amount of risk offers the highest expected return), and at the **tangent point of the CAL** (offers the highest return per unit of risk).  
#### Beta Calculation
Betas are estimated using historical data, but in the end we want a forward looking beta estimate.
- Based on a regression analysis using stock and market returns

```ad-important
**Definition 7.2**: Single-index Market Model 

The single-index market model represents a direct way to estimate the beta of an asset or security using historical information:
$$\beta_{it}=\alpha_{i}+\beta_{i}R_{mt}+\epsilon_{it}$$

The market model disaggregates a company’s stock return into two components:
1. Stock return explained by movements in the market index
2. Stock return **NOT explained** by the movements in the market index, $\alpha_{i}$

A extremely efficient CAPM model has that $\epsilon_{it}=0$ for all asset $i$.
```

Most stocks have betas in the range of 0.5 to 1.5.

#### CAPM, Assumptions

```ad-important
**Definition 7.1**: CAPM, Assumptions

1. Investors are **risk averse** and **maximize expected utility **of end-of-period wealth,  
2. Investors are price takers with **homogeneous expectations** about asset returns,  
3. A **risk-free asset exists** and investors can borrow or lend money at the risk-free rate,  
4. Asset quantities are fixed, assets are **marketable** and **divisible**, and
5. Asset markets are **frictionless**: same information, no taxes, regulations and short-selling restrictions.
```

![[Pasted image 20240208132018.png|500]]

Note that **CML** (Capital Market Line) and **Market Portfolio** are equivalent to CAL and Tangency Portfolio in Portfolio theory, respectively.

---
### CAPM, Implications 
Note that since every investor faces the same CML in equilibrium, all investors should only hold combinations of the market and the risk-free asset.
- **Risk averse investors** hold a larger portion of their assets in the **risk free asset** and a smaller portion in the market portfolio
- **Risk tolerant investors** hold a smaller portion of their assets in the risk free asset and a larger portion in the **market portfolio**

The implication is that all investors will hold the **same portfolio** for risky assets: the "market portfolio". Investors should only be awarded for **holding systematic risk**.

![[Pasted image 20240208132907.png|500]]


In the "CAPM world", there is no such thing as  overpricing/underpricing. All assets are fairly priced.
- However, we can compare an asset’s given price or expected return relative to what it  should be according to the CAPM, and in that context we talk about over/under pricing
	- Assets above the SML are underpriced relative to the CAPM.  
	- Assets below the SML are overpriced relative to the CAPM