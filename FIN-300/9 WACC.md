[[2023-04-12]] #Investment #Valuation 

### Weighted Average Cost of Capital
The WACC definition is consistent with our **free cash flow** definition.

Recall how free cash flow is calculated [[4.1 Capital Investment - Valuation#^ea68e9|in Section 4.1]].
- We want to **strictly follow** this model to avoid mistakes in WACC calculation

```ad-important
**Definition 9.1**: WACC
The Weighted Average Cost of Capital can be expressed as $$WACC=\text{Cost of Equity}\cdot \frac{EQV}{EV}+\text{Cost of Debt}\cdot(1-T)\cdot \frac{DV}{EV}$$

where $EQV$ is the **value of equity**, $DV$ is the **value of debt**, and $T$ is corporate tax rate.
```

^335717

```ad-note
Note that $\frac{EQV}{EV}=\frac{EQV}{DV+EQV}=1-\frac{DV}{EV}$
```

To compute the above, we need
- Discount rate - given by **YTM of 10-year bonds**
- Free cash flow - found in **accounting statements**
- [[6.2 Stocks II#^794373|Estimate of $EV$]]: PV of projected cash flow + the value of non-operating assets
- [[8.2 CAPM#^b3afc2|Cost of Equity]], also interpreted as **expected return of security** - given by **risk-free rate, market risk premium and equity beta** - all found from comparable firms

#### Computation
In actual computation, there could be two scenarios:
1. Do not know the value of debt but does have an estimate of **percentage leverage**: $\frac{DV}{DV+EQV}$
	- In this case, we estimate the WACC with **reference to other firms** with a similar risk profile (Given)
	- Then, we compute **enterprise value**
	- Finally, we can know the number of **debt** by multiplying percentage leverage by $EV$ since $DV+EQV=EV$

2. Know the value of debt
	- First, make an assumption about **input leverage** in the WACC
	- Then, we compute **enterprise value**
	- Then, we can compute the **output leverage** as now we have an estimate of $EV$
	- Since the input leverage differs from the output leverage, we compute the **difference**
	- Use `goalseek()` in Excel to set the difference to zero