[[2024-02-13]] #Investment #Portfolio 

CAPM often fails empirically. Investors are more concerned about knowing what **factors** determine expected returns and what risks are priced in the market.
- Roll’s critique: it is difficult to obtain the **empirical market portfolio**
- Systematic risk is not due to one source, which implies a **multi-factor model**

### APT Model 
The arbitrage pricing theory was created by Stephen Ross and it is similar to the CAPM but it requires fewer [[7 CAPM#^cec56e|assumptions]]. It is a **multi-factor model** of asset pricing: more than one source of undiversifiable risk explains security returns.

```ad-important
**Definition 8.1**: Arbitrage

Arbitrage arises if an investor can construct a zero-investment portfolio with a sure profit. Since no net investment outlay is required, an investor can create arbitrarily large positions to secure large levels of profit.
```

In a well-functioning market (efficient market) such profitable arbitrage opportunities will **quickly disappear** as a result of supply and demand pressure.

```ad-example
The following strategy/opportunity should not persist in competitive capital markets.

![[Pasted image 20240213131532.png|500]]
```

```ad-important
**Definition 8.2**: APT Assumptions 
1. All securities have **finite expected returns and variances**,  
2. Some investors can form well-diversified portfolios,  
3. There are **NO** market “**frictions**” (taxes, transaction costs, etc.), and
4. **NO arbitrage** opportunity exists among well-diversified portfolios. If any arbitrage opportunities do exist, they will be exploited away by investors.  
```

Sources of risk and uncertainty that are systematic and cannot be eliminated by diversifying would affect asset prices. These systematic risks are called **factors**. 

A factor is a variable which influences the returns of assets. **Exposure to factor risk** over the long run yields a **risk premium**.
- Risk premium is the compensation of being exposed to the factor results in holding risk that other investors seek to avoid

Types of factors include
- Macro factors: GDP growth, inflation 
- Style factors: value-growth, momentum, low volatility

```ad-important
**Definition 8.3**: Arbitrage Pricing Theory (APT)

For a **well-diversified portfolio**, a basic relation describing arbitrage pricing theory can be written as the following $n$-factor model:
$$\mathbb{E}(R_p)=R_{f}+\sum\limits_{i} \beta_{i}RP_{i}$$
where $\beta_{i}$ is the sensitivity (loading) to the $i$th factor and $RP_{i}$ is the risk-premium on the $i$th factor. And,
$$R_{p}=\mathbb{E}(R_{p}) + \sum\limits_{i} \beta_{i}f_{i} $$

where $f_{i}$ is the **unanticipated change** in factor $i$ and $\beta_{i}f_{i}$ describes the total surprise.

The actual return depends on the specific factor model and the corresponding factor surprises $f_{i}s$.
```

The APT implies that **ONLY systematic risk should be rewarded**: the idiosyncratic (non-systematic) risk can be diversified away. 

```ad-note
APT does **NOT** specify the factors.
```

```ad-summary
**APT versus CAPM**
- **CAPM**
	- Based on an inherently unobservable **“market” portfolio**
	- Provides an unequivocal statement on the **expected return-beta relationship** for all securities
- **APT**
	- Built on the foundation of **well-diversified portfolios** 
	- **CANNOT** rule out a **violation of the expected return-beta relationship** for any particular asset.
	- Does **NOT** assume investors are mean-variance optimizers
	- Uses an observable market index
```

```ad-summary
**APT Drawbacks**
The drawback of the APT model is that it does not specify the systematic factors. It is also more difficult to apply, as it takes a considerable amount of time to determine all the various factors that may influence the price of an asset.
- Most investment firms have their own preferred set of factors and corresponding factor sensitivities
```
