[[2023-01-23]] #CashFlow #DCF #Annuity #Perpetuity

### Multiple Cash Flows
Investments typically generate **multiple** cash flows.
- The value of an investment is the **present value** of all its **expected** **cash flows**.
	- Expectation is probability-weighted

```ad-example

We calculate the present value for each investment, and sum them up accordingly.

![[Pasted image 20230123115845.png|600]]
```

In this application, the cash flows are (usually) equally-spaced (1 year),  so we can use the Excel function `NPV (discount rate, cash flows)`.
-  However, we need to manually input cash flow at time `0`.
	- `=NPV(discount_rate, cash flows 1-11) + year0_cash_flow`

---

### Annuity, Perpetuity
An annuity is a **stream of projected cash flows** that grow at a **constant rate** each year (the growth rate can be zero).

A **perpetuity** is a type of **annuity** with **NO END**.  Perpetuity is a financial term that indicates an **infinite stream** of cash flows.

Instead of computing the present value of each **individual cash flows**, there is an equation for the **present value** of all **ALL** flows in the **annuity**.

$$PV=\frac{C_1}{(1+r)}+\frac{C_1\cdot(1+g)^1}{(1+r)^2}+\cdots+\frac{C_{1\cdot}(1+g)^{T-1}}{(1+r)^T}$$

$$
=\frac{C_{1}}{r-g}\cdot \left(1-\left(\frac{1+g}{1+r}\right)^T\right)
$$

When $T\to \infty$, $PV=\frac{C_1}{r-g}$.

```ad-note
Note here $C_1$ refers to the cash flow at the **END** of period $1$, that's why we accounted for the discount rate.
```

