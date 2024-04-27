[[2024-04-26]] #Bonds #Options #Valuation 

### Overview
**Call** option **benefits the bond issuer** (most prevalent). 
- In contrast to bonds issued in the 1990s, most investment-grade corporate bonds have a make-whole call provision, in which the call price compensates the bondholder for surrendering the bond
- The call price is determined at different points in time by **discounting remaining coupon payments and face value repayment** at an interest rate which is a small spread above Treasury yields
- American, European, Bermudan (specified possible redemption dates)

**Put** option **benefits the bond holder**.
- American, European 

**Extension** option **benefits the bond holder**. **Conversion** to equity option **benefits the bond holder**.

---
### Valuation
The value of a **callable bond** is the value of a **straight bond** - the value of the **issuer call option**.
- Value the **straight bond** by **discounted coupons and face value at an estimate of YTM** OR by using a **binomial tree**
- Value the callable bond using a **binomial tree**
- Work out the value of the call option using the difference between the two

The value of a **puttable bond** is the value of a **straight bond** + the value of the **investor put option**.
- Work out the valuation in similar fashion as for callable bonds

#### Forward Rates
A forward rate is **a projection of future interest rates** calculated from either spot rates or the yield curve. 

We are trying to find the future interest rate $r_{1,2}$ for time period $(t_{1},t_{2})$ given the rate $r_{1}$ for time period $(0,t_{1})$ and rate $r_{2}$ for time period $(0,t_{2})$. To do this, we use the property that the proceeds from investing at rate $r_{1}$ for time period $(0,t_{1})$ and then **reinvesting** those proceeds at rate $r_{1,2}$ for time period $(t_{1},t_{2})$ is **EQUAL to** the proceeds from investing at rate $r_{2}$ for time period $(0,t_{2})$. Mathematically, we have
$$(1+r_{1})^{t_{1}}\cdot(1+r_{1,2})^{(t_{2}-t_{1})}=(1+r_{2})^{t_{2}}$$
Rearranging gives 
$$r_{1,2}=\left(\frac{(1+r_{2})^{t_{2}}}{(1+r_{1})^{t_{1}}}\right)^{1/(t_{2}-t_{1})}-1$$
Or, in terms of discounting factors $\text{df}$,
$$r_{1,2}=\left(\frac{\text{df}_{0,t_{1}}}{\text{df}_{0,t_{2}}}\right)^{1/(t_{2}-t_{1})}-1$$
