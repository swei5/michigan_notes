#Bonds #Valuation 

### Overview
Transaction in a given period (at an **instance**) is **DIFFERENT** from transactions **over a given time frame** (the time dimension involved).

```ad-example
Assume two streams of payments:
$$\pi_1=\set{1,0,0}$$
$$\pi_2=\set{0,1,0}$$

Case 1:
- If $\pi_1$ is acquired
	- Consume on Day 1
	- Store it since Day 1
- If $\pi_2$ is acquired
	- **ONLY** able to consume on Day 2
	- **ONLY** able to store it since Day 2

Therefore, $\pi_1$ is superior to $\pi_2$.

We can generalize the problem to, for what values of $y$ does
$$\pi_i=\set{1,0,0,\cdots,0}\equiv \pi_j=\set{0,0,0,\cdots,y,\cdots,0}$$
```

#### Nominal Interest Rate

```ad-important
**Definition 3.1**: **Interest Rate (Discount Rate)**

Nominal Interest Rate $i$ is an important economic factor that can be expressed as
$$i=i_{p}+i_{DF}+i_{\pi}$$

```

- $i_{p}$ is patience
	-  Consumption in the future should be adjusted upwards because of the longer wait
- $i_{DF}$ is default risk
	- Should be compensated for any risks that might be incurred
- $i_{\pi}$ is inflation
	- A higher return is expected due to inflation

---

### Debt Instruments
#### Simple Loans
Lender receives the principal amount plus a fee at maturity. Trivial.

![[Pasted image 20230302210845.png|500]]

#### Discounted bond
Acquiring bond at a value smaller than the value of the bond in the future.

![[Pasted image 20230302210903.png|500]]

#### Coupon Bond
As the bond-holder, one will receive a series of periodic payments - **coupon payments** until the bond matures, at which the **par value** of the bond will also be paid.

![[Pasted image 20230302210953.png|500]]

The price of a coupon bond is the sum of values of all the **future payments**. As we have seen in Finance lecture, ![[5.1 Bonds I#^4ca547]]
Thus,
$$P=\frac{C}{r}\cdot\left(1-\left(\frac{1}{1+r}\right)^{T}\right)+\frac{FV}{(1+r)^T}$$

Here, $r$ is also known as the **yield to maturity (YTM)**, which equates the present value of the payments from an asset with the asset’s price today. In this scenario, this is also interchangeable with the term **coupon rate**.

```ad-important
**Coupon rate** is a fixed ratio that **DOES NOT change** with market conditions - therefore the interest rate risks.
```

```ad-note
We note that $P=FV \iff r=\frac{C}{FV}$.
- This implies when a bond is trading **at par**, its **yield** is equal to its **coupon rate**. Matches with our observation [[7 Bonds I#^5a2fcb|here]].


More interestingly, we note that the valuation of a bond $P$ isn't a function of $t$.
- The valuation of the bond **DOES NOT** change as long as market condition does not change

In simple terms, the price of the bond does not change overtime because increased coupon payments due to longer maturity date are **offset by the discounted face value**, ceteris paribus.
```

```ad-info
**Derivation**:

![[Pasted image 20230302211313.png|500]]
```

#### Fixed Payment Loan
This is effectively a coupon bond except that we exchange the repayment of the face value by $T-1$ equal payments.
- This is the standard mortgage model

![[Pasted image 20230302220324.png|500]]

Given per period installment $x$ and nominal interest rate $i$, the value of the loan over a period of $T$ is thus
$$P=\frac{x}{1+i}\left(\frac{1-\frac{1}{(1+i)^T}}{1-\frac{1}{1+i}}\right)=\frac{x}{i}\left(1-\frac{1}{(1+i)^T}\right)$$

This is very similar to coupon bonds, with the absence of the final payment of face value at time $T$.

Equivalently, given a loan amount of $L$, the per period installment is $x=\frac{iL}{1-\frac{1}{(1+i)^T}}$. The derivation looks similar to that of a coupon bond.

```ad-note
Note that all valuation methods discussed above come down to the [[2.2 Discounted Cash Flow II#^384322|simple discounted cash flow model]].
```

Alternatively, the outstanding loan given time $t$ can be expressed in an iterative fashion:
$$L_t=L_{t-1}+iL_{t}-x$$
Normally, $x>iL_t$ as it should cover the interest payment and **partially pays off the principal**.
- If one only wants to pay off the interest (leaving the value of the loan unchanged), then $x=iL_t$

In the case of **deferred payments** for $d$ periods, the per period installment $x$ is then
$$x=\frac{iL(1+i)^{d}}{1-\frac{1}{(1+i)^T}}$$
Here, the term $(1+i)^d$ applied on the numerator is accounted for the **inflation** factor of accumulated interest during the $d$ periods in which payments are deferred. Note that this is equivalent to taking out a bigger loan at a later time.

```ad-warning
**Yield to maturity** is a different concept from **yield**.
```

---
### Perpetuity
Through a closer examination of the pricing of coupon bonds
> $$P=\frac{C}{r}\cdot\left(1-\left(\frac{1}{1+r}\right)^{T}\right)+\frac{FV}{(1+r)^T}$$

We note that as $T\to \infty$, $(1+r)^{T}\to 0$  and hence $\frac{FV}{(1+r)^{T}}\to 0$. This makes the entirety of the bond value $P=\frac{C}{r}$. This is known as the **price of perpetuity**.

