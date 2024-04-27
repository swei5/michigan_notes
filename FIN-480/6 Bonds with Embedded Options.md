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

The valuation steps are as followed:
1. Model interest rate shocks
2. Model bond values
3. Determine whether the option is exercised
4. Work backwards through the tree 

#### Forward Rates
A forward rate is **a projection of future interest rates** calculated from either spot rates or the yield curve. 

We are trying to find the future interest rate $r_{1,2}$ for time period $(t_{1},t_{2})$ given the rate $r_{1}$ for time period $(0,t_{1})$ and rate $r_{2}$ for time period $(0,t_{2})$. To do this, we use the property that the proceeds from investing at rate $r_{1}$ for time period $(0,t_{1})$ and then **reinvesting** those proceeds at rate $r_{1,2}$ for time period $(t_{1},t_{2})$ is **EQUAL to** the proceeds from investing at rate $r_{2}$ for time period $(0,t_{2})$. Mathematically, we have
$$(1+r_{1})^{t_{1}}\cdot(1+r_{1,2})^{(t_{2}-t_{1})}=(1+r_{2})^{t_{2}}$$
Rearranging gives 
$$r_{1,2}=\left(\frac{(1+r_{2})^{t_{2}}}{(1+r_{1})^{t_{1}}}\right)^{1/(t_{2}-t_{1})}-1$$
Or, in terms of discounting factors $\text{df}$,
$$r_{1,2}=\left(\frac{\text{df}_{0,t_{1}}}{\text{df}_{0,t_{2}}}\right)^{1/(t_{2}-t_{1})}-1$$

```ad-example
**Example, Valuation of Callable bond in absence of interest rate volatility**

![[Pasted image 20240426212422.png|600]]

Note that the value of the straight bond depends on cumulative discount factors, which depend on forward rates observed at the beginning. 

The value is the minimum of the two because the issuer would want to redeem the bond at the lowest cost possible.
```

#### Modeling Interest Rates 
Interest rates are volatile, and changes in interest rates determine the future price of bonds. **More volatility** in interest rates means **more bond price volatility** and therefore **more value to the holder of a call or put option**. 

Bond valuation process is the same as for securities we previously considered, with the only added part being that the value of the straight bond at each point in time is **a function of interest rate movements**.

We assume **lognormal changes** in interest rates, **NO negative rates**, and **higher volatility at higher interest rates**. And, the interest rate in the **up state** ($r_{u}$) can be modeled by the interest rate in the **down state** ($r_{d}$) by
$$r_{u}=r_{d}\cdot e^{2\sigma \sqrt{\Delta t}}$$
We can then solve for $r_{u}$ using a binomial tree, including making the following simplifying assumptions:
- A bond is **[[5.1 Bonds I#^5a2fcb|valued at par]]**
- There is an equal chance of an increase or decrease in rates 
- $r_{d}$ can be obtained via goal seek