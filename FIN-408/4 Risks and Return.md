[[2024-01-23]] #Risks #Investment 

### Rate of Return

```ad-important
**Definition 4.1**: Rate of Return 

The rate of return (RoR) is used to measure the profit or loss of an investment (relative to the initial investment) over time and it is defined as:
$$\text{RoR}=\frac{\Delta\text{Balance}}{\text{Balance}_0}$$

See [[7.1 Investment Returns#^167611|total return]] for a more specific definition.
```

In calculating rate of return, we must take into account both **dividend yield** and **capital gain** made on the investment.

#### Nominal versus Real Return 
A **nominal return** represents the growth rate of your money.  A **real return** represents the growth rate of your **purchasing power**.

Let $i =$ nominal rate, $r =$ real rate and $\pi =$ inflation rate. Then
$$1+r = \frac{1+i}{1+\pi}$$$$\implies r \approx i-\pi$$
When the nominal rate and the inflation rate is *sufficiently small*. Note that this is from the [[4.2 Capital Investment - Interest Rate#^2fe11c|Fisher Equation]].

#### Expected Return 
Defined as
$$\mathbb{E}(R_{t,t+1})=\frac{\mathbb{E}(P_{t+1})+\mathbb{E}(D_{t+1})}{P_t}-1$$

Where $P_t$ is prices at time $t$ and $D_t$ is dividends paid at time $t$. From that we could define the **risk premium**: ![[7.2 Risk and Return Profile#^419f8c]]
Read [[8.1 Portfolio Theory#Expected Returns|Expected Returns]] and [[8.1 Portfolio Theory#Portfolio Risk and Return|Portfolio Risk and Return]] for more information related to calculating expected return and risks (variance) given some random probability distribution. Note that 
$$\text{Var}(X,Y)=\mathbb{E}\left(X^{2}\right)-\left(\mathbb{E}(X)\right)^{2}$$

Read [[8.1 Portfolio Theory#Covariance and Correlation|Covariance and Correlation]] for information related to computing the correlation coefficient and covariances between assets. Note that
$$\text{Cov}(X,Y)=\mathbb{E}(XY)-\mathbb{E}(X)\mathbb{E}(Y)$$

Recall the definition of the Sharpe Ratio: ![[7.2 Risk and Return Profile#^3624b7]]
Historically, the sharpe ratio of US equities is around 0.30.

---
### Statistics 
Use the **arithmetic mean return** if you want to make an **estimate of future returns** based upon a sample of past returns, and you consider that the series of returns is **independent**.

Use the **geometric mean** return if you want to report to investors the **actual returns on a periodic basis**.
- E.g. CAGR 

Greater variance of returns of a security means greater uncertainty (risks). Securities with **more volatile returns** tend to have **higher average returns**. This is known as the risk-return tradeoff.

Standard deviation of the return on a stock is one measure of risk of investing in that stock.

```ad-summary
**Two fundamental questions in finance**:
1. What is risk and how should we measure it?  
2. How much extra expected return do we need to be compensated for the **additional risk**?
```
