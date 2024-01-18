[[2024-01-11]] #Options #Valuation 

### Option Concepts, Overview
- **Option types**  
	- Call: right to buy  
	- Put: right to sell
- **Contract terms**  
	- **Strike price**: AKA exercise price
	- Time to expiry  
	- [European vs American](https://en.wikipedia.org/wiki/Option_style)
		- **European**: can only exercise on the specified date
		- **American**: can exercise up until the specified date
	- Underlying asset  
- **Other**  
	- **Spot price**: price of the underlying asset 
	- Risk-free rate
	- Buy: long
	- Sell: write, short
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
$$p=\max(s_{T}-x,0)$$
And the payoff to the seller is
$$p=-\max(s_{T}-x,0)$$
Where $s_T$ is the underlying asset price at expiration date $T$ and $x$ is the exercise price of the option.

#### Put Options 
Buyer has the option to sell an underlying asset to the seller at a fixed exercise price. Note that this is the reverse of a call option. Selling a put option is also risky, because the underlying asset price could move to a very low price.

Payoff to the buyer is
$$p=\max(x-s_T,0)$$
And in turn the payoff to the seller is
$$p=-\max(x-s_T,0)$$

Our valuation approach is to estimate the **risk-neutral expected payoff** and **discount** to time zero at the **risk-free rate of interest**.

```ad-info
**Expected payoff** is a **weighted average** of all possible payoffs (cases).
```

```ad-note
Note that in project evaluations, we often used a risk-adjusted rate as the discounted factor (WACC, interest rate, etc.).

However, in option valuation, we factor in risk-adjustment by determining the expected payoff **using risk-neutral probabilities**. This is because option is a much riskier asset than stocks, which could go up to 30% to 50%.
- Expected rate of return (discount rate) is impossible to know unless we know the actual distribution of the prices
```

The approach to calculate the risk-neutral probabilities is beyond the scope of this lecture.

```ad-warning
We will always use a **RISK-FREE rate of interest** in discounting the expected cashflow.
```

#### Valuation Steps
1. Estimate the possible prices of the underlying asset (spot price) at the expiration date.
	- This is usually given by a probability distribution
2. Estimate the **risk-neutral expected future value of the underlying asset**
	- This is simply the future value of the underlying assets given some risk-free rate 
3. Based upon the **risk-neutral expected future value** of the underlying asset, compute the risk-neutral probabilities of high and low outcomes
	- $pH+(1-p) L=\mathbb{E}(S_{T})$: solve for $p$: $p=\frac{\mathbb{E}(S_{T})-L}{H-L}$
1. Compute the **option payoffs** at the expiration date
2. **Apply the risk-neutral probabilities** of high and low outcomes, compute the risk-neutral expected future value of the option
3. Discount the expected future value of the option to time zero at the risk-free rate of interest

```ad-note
The traditional valuation follows the Black-Scholes formula, but the binomial tree valuation method provides more flexibility and is simpler.
```