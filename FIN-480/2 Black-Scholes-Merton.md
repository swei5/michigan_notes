[[2024-01-25]] #Options #Valuation 

### Assumptions 
The model prescribes the assumptions below:
1. Underlying asset values take on a lognormal distribution
	- Return $\to \ln\left(\frac{P_{t}}{P_{t-1}}\right)$
	- In a lognormal distribution of asset values, **continuously compounded returns** are a **normal distribution**
3. Investors can borrow and lend at the **continuously-compounded risk-free rate**
	- **Continuously-compounded risk-free rate** defined as $\ln(1+EAR)$
	- E.g. if $EAR=2\%$, then the continuously-compounded risk free rate is $\ln (1.02)=1.98\%$
1. No cash flows (e.g. dividends) before option expiry 
2. European options

### Equation 
The call option value is given by
$$C=SN(d_{1})-Xe^{-rT}N(d_{2})$$
Where $S$ is the **underlying asset price** today, $X$ is the **exercise price**, $N (\cdot)$ is a function returns the **area under a normal curve**, $r$ is continuously-compounded risk-free rate, and $T$ is the time to expiry in years.

$d_1$ is defined as
$$d_{1}:=\frac{\ln\left(\frac{S}{X}\right)+T\left(r+\frac{\sigma^{2}}{2}\right)}{\sigma \sqrt{T}}$$
Where $\sigma$ is the **volatility** of **continuously-compounded returns** on an **annual basis**. And $d_{2}$ is defined as
$$d_{2}:=d_{1}-\sigma \sqrt{T}$$

Conversely, the put option value is given by
$$P=Xe^{-rT}N(-d_{2})-SN(-d_{1})$$

The range of $d_1$ and $d_2$ is infinite, but mostly within $[-3,3]$.

```ad-note
Here, $N(d_{1})$ accounts for both the probability that the **underlying asset price finishes above the exercise price**, and when this happens, **how far above** the exercise price the underlying asset price will be.
```

If a [[1 Options, Binomial Tree Valuation#^115aa9|binomial tree]] is constructed under the same assumptions as the Black-Scholes-Merton model, its **valuation will converge** to the BSM evaluation if $t\to 0$.