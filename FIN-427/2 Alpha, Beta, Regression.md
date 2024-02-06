[[2024-01-23]] #Investment 

We use several metrics to assess portfolio performance.
- Total returns vs benchmark  
- Standard deviation (monthly and annualized)  
	- Measure of diversion from main not necessarily a good benchmark 
- [[7.2 Risk and Return Profile#^3624b7|Sharpe ratio]]  
- Alpha

### Return and Volatility Summary 
**Arithmetic mean returns** used as an **estimate of future returns** when the past returns distribution is considered an unbiased estimate of the future returns distribution.
- Used in **expectation of the future return** based on historical data

Recall that ![[7.1 Investment Returns#^a7f89a]]

**Geometric mean returns** show what an investor actually earned. Recall that ![[7.1 Investment Returns#^0bce4e]]

**Discrete time return** is defined as
$$R=\frac{FV}{PV}-1$$ which is what investors mostly use. Whereas **continuously-compounded return** is defined as
$$R=\ln\left(\frac{FV}{PV}\right)$$

To convert discrete time returns to annualized returns, we use
$$\overline{R}_{\text{Annual}}=(1+\overline{R}_{\text{Monthly}})^{12}-1$$

And **standard deviation of returns annualized** is defined as:
$$\sigma_{\text{Annual}}=\sqrt{\left(\sigma_{\text{Monthly}}^{2}+(1+\overline{R}_{\text{Monthly}})^{2}\right)^{12}-(1+\overline{R}_{\text{Monthly}})^{24}}$$
Where $\overline{R}_{T}$ is the arithmetic or geometric mean of a series of return over time $T$.

---
### Volatility and Sharpe Ratio 
The **[[7.2 Risk and Return Profile#^3624b7|Sharpe ratio]]** is the slope of the line between two points: a proxy for the risk-free rate and a portfolio.

The term **excess return** means return minus the risk-free rate. The term **abnormal return** means return minus a *fair return for risk*, often measured using an index of estimated equivalent risk.

```ad-note
The Sharpe ratio is typically estimated with wide margin for error because portfolio volatility is high and the time series is short (low signal to noise ratio).

Hence, buying funds merely because they have a high Sharpe ratio will not  
necessarily lead to high future performance.
```

---
### Factor Model
Regressing excess returns of the fund against excess returns of a midcap index generates information allowing us to measure exposure to the “midcap factor” and, on average, alpha (the intercept, which represents average monthly returns from industry- and company-specific **deviations from the benchmark**). The empirical model is
$$R_{1,t}=\alpha+\beta R_{0,t}+\epsilon_t$$
Where $R_{i,t}$ is the excess return of asset $i$ and $\epsilon_t$ is the error of the model in month $t$.

![[Pasted image 20240123154641.png|500]]

#### Interpreting Output 
- **Durbin-Watson**: time series **correlation of errors**
	- If errors are correlated the standard error will be understated
- **Kurtosis**: a measure of the **tailedness** of a distribution
- **Jarque-Bera**: a goodness-of-fit test of whether sample data have the skewness and kurtosis matching a normal distribution
	- Lower probability of JB means residuals are **NOT normally distributed** and one should check for outliers

Outliers are important - the measurement of alpha and beta changes markedly with a few data points.

```ad-important
Dealing with **influential observations** is more important than formal statistical tests.
```

