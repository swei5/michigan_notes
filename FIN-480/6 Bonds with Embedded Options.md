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

The above discussion mainly concerns **government bonds** - which are risk-free instruments. In consideration of corporate bonds, a **spread** needs to be applied to the estimated interest rate **AT EACH STAGE**.
- This decreases the PV of the bond since investors demand a higher compensation

```ad-example
**Example, 3-Step Binary Tree**

![[Pasted image 20240427005908.png|600]]

We start modeling from the right by first **inputting the future values of the bond for the 3-year bond** ($1,036). This value should be **same across** all scenarios since it only depends on the coupon rate (par yield).

The forward rates of the second year (3.85%, 3.15%, etc.) are derived from the formula above with goal seek. The bond values of the second year (up and down scenarios) are the corresponding values from the third stage, **discounted at the corresponding forward rate**, **plus a coupon payment**. The coupon payment should be the same across all scenarios and across all stages, since it is only dependent on FV ($1,000) and coupon rate (3.60%).

The bond values of the second year is **an average** between the up and down scenarios. The forward rates of the first year is computed through solving a sub-problem with a 2-year bond using the same approach.
```

#### Modeling Embedded Call Option 
Now that we have modeled the interest rates, let’s value a bond with an embedded call option. To answer this, we first calculate, in the absence of call provision, what the bond is worth $P_{i}$ at period $i$. Then, we compare $P_{i}$ to the exercise price $X$ and ask, what is the **minimum value associated** with alternative courses of action? We select the minimum of the two be the value of the bond at stage $i$, for a given stage.

```ad-note
Note that the exercise price does **NOT** include the coupon payment. Hence, we should always compare the option exercise price with the value **NET the coupon payment** of the bond at time $i$.
```

After that, we discount the values of bonds at each stage to present to arrive at the fair value of the bond.

![[Pasted image 20240427215248.png|600]]

Alternatively, we may use cumulative discount factors to analyze this in a tabular form.

In the case of a **put option**, we use the `MAX()` function as investors aim to sell the bond at the maximum price possible.