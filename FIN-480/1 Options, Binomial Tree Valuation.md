[[2024-01-11]] #Options

### Option Concepts
- **Option types**  
	- Call: right to buy  
	- Put  
- **Contract terms**  
	- **Strike price**: exercise price
	- Time to expiry  
	- European vs American
		- **European**: can only exercise on the specified date
		- **American**: can exercise up until the specified date
	- Underlying asset  
- **Other**  
	- **Spot price**: price of the underlying asset 
	- Risk-free rate  
	- Buy/long vs sell/write/short  
	- **Option premium**: price of the option
- **Valuation**  
	- Moneyness  
		- **At the money**: when spot price is equal to strike price
		- **In the money**: when there is immediate **positive cash pay-off** by exercising the option
			- E.g. When strike price is lower than the spot price for a **call option**
		- **Out of the money**: when there is immediate **negative cash pay-off** by exercising the option
	- Intrinsic value versus time value
		- **Intrinsic value**: immediate cash pay-off by exercising the option 
		- **Time value**: **premium** in excess of intrinsic value before expiration
		- Option premium is often higher than the intrinsic value of the option because of the time value of an option
	- Binomial tree  
- **Underlying assets**  
	- Stock  
	- Commodities  
	- Currencies  
	- Interest rates 

---
### One Step Binomial Tree Valuation 
#### Call Options
Buyer has the option to buy an underlying asset from the seller at a fixed exercise price. **However, selling a call option is risky**, because the underlying asset price could move to a very high price.
- **Naked call option**: Seller doesn’t own underlying
- **Covered call**: option: Seller does own underlying

Payoff to buyer is thus
$$P=\max(s_{T}-x,0)$$
And the payoff to the seller is
$$P=-\max(s_{T}-x,0)$$
Where $s_T$ is the underlying asset price at expiration date $T$ and $x$ is the exercise price of the option.

#### Put Options 
TBD

Our valuation approach is to estimate the **risk-neutral expected payoff** and **discount** to time zero at the **risk-free rate of interest**.

```ad-info
**Expected payoff** is a **weighted average** of all possible payoffs (cases).
```

```ad-note
Note that in project evaluations, we often used a risk-adjusted rate as the discounted factor (WACC, interest rate, etc.); however, in option valuation, we factor in risk-adjustment by determining the expected payoff using risk-neutral probabilities. This is because option is a much riskier asset than stocks, which could go up to 30% to 50%.
```

The approach to calculate the risk-neutral probabilities is beyond the scope of this lecture.
