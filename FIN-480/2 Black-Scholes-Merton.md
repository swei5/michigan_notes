[[2024-01-25]] #Options #Valuation 

### Assumptions 
The model prescribes the assumptions below:
1. Underlying asset values take on a lognormal distribution
	- Return $\to \ln\left(\frac{P_{t}}{P_{t-1}}\right)$
	- In a lognormal distribution of asset values, **continuously compounded returns** follow a **normal distribution**
3. Investors can borrow and lend at the **continuously-compounded risk-free rate**
	- **Continuously-compounded risk-free rate** defined as $\ln(1+EAR)$
	- E.g. if $EAR=2\%$, then the continuously-compounded risk free rate is $\ln (1.02)=1.98\%$
1. No cash flows (e.g. dividends) before option expiry 
2. European options

### Equation 
The call option value is given by
$$C=SN(d_{1})-Xe^{-rT}N(d_{2})$$
Where $S$ is the **underlying asset price** today, $X$ is the **exercise price**, $N (\cdot)$ is a function returns the **area under a normal curve** $N(0,1)$, $r$ is **continuously-compounded risk-free rate**, and $T$ is the time to expiry in years.

$d_1$ is defined as
$$d_{1}:=\frac{\ln\left(\frac{S}{X}\right)+T\left(r+\frac{\sigma^{2}}{2}\right)}{\sigma \sqrt{T}}$$
Where $\sigma$ is the **volatility** of **continuously-compounded returns** on an **annual basis**. And $d_{2}$ is defined as
$$d_{2}:=d_{1}-\sigma \sqrt{T}$$

Conversely, the put option value is given by
$$P=Xe^{-rT}N(-d_{2})-SN(-d_{1})$$

The range of $d_1$ and $d_2$ is infinite, but mostly within $[-3,3]$.

```ad-note
$N(d_{1})$ measures the **delta** of a call option, and $N(d_{2})$ measures the **delta** of a put option.
```

If a [[1 Options, Binomial Tree Valuation#^115aa9|binomial tree]] is constructed under the same assumptions as the Black-Scholes-Merton model, its **valuation will converge** to the BSM evaluation if $t\to 0$.

---
### BSM with Dividends 
The dividend yield is incorporated in the equation by **adjusting the drift of the underlying asset downwards**. That is, rather than the underlying asset drifting upwards at the risk-free rate, it drifts upwards at the risk-free rate less the dividend yield.

After the addition:

$$d_{1}:=\frac{\ln\left(\frac{S}{X}\right)+T\left(r-q+\frac{\sigma^{2}}{2}\right)}{\sigma \sqrt{T}}$$
Where $q$ is the **continuously-compounded dividend yield**. And, 
$$C=Se^{-qT}N(d_{1})-Xe^{-rT}N(d_{2})$$
$$P=Xe^{-rT}N(-d_{2})-Se^{-qT}N(-d_{1})$$

---
### Sensitivity 
- $\nu$ (Vega) : sensitivity to **volatility assumption**
	- A call and put with the same exercise price have the same exposure to volatility
- $\delta$ (Delta): sensitivity to **underlying price** 
	- More in the money, higher delta
- $\theta$ (Theta): sensitivity to **time**
	- More in the money, higher theta
	- Closer to expiration, higher theta
- $\rho$ (Rho): sensitivity to the **risk-free rate**
	- More in the money, higher rho

```ad-summary
**Comparison between BSM and Binomial Tree**
- BSM 
	- Accounts for **all possible** underlying asset price movements
	- Only works with **European options**
	- More accurate if asset prices are lognormally distributed
- Binomial Tree 
	- Accounts for **early exercise**
```

**Real options** analysis must be done using a binomial tree and not the Black-Scholes-Merton equation because the assumptions underlying the BSM equation are very far away from a real project.