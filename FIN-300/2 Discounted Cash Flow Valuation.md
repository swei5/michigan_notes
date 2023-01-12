[[2023-01-09]]

### Time Value of Money
- A dollar today is worth **MORE** than a dollar tomorrow.
- **CORE PRINCIPLE** in finance
- The **difference** in **value** between money today and money in the future is the **TVM**.

```ad-question
Due to the TVM, how do we compare cash at different points in time?
```

---
### Interest Rates, Present Value, Future Value

#### Interest Rate
We can use the **interest rate** to calculate the Present Value of the money you
receive in the future, or the Future Value of the money you receive today.

```ad-info
**Definition 2.1**: To compare the value or money from the future to the value today, we need an **exchange rate** across time. This is **discount rate** or **interest rate**.
```

---

#### Two Scenarios: Present Value and Future Value
- How much is a future unit of cash worth today?
- How much is a present unit of cash worth in the future?
![[Pasted image 20230109121422.png|400]]

We relate the present value, future value, and the per-period interest rate with the following equation:

$$FV\ in \ n \ periods=PV(1+r)^n$$

Rearranging gives:

$$PV=\frac{FV\ in \ n \ periods}{(1+r)^n}$$

```ad-note
Future cash flows are **discounted** at the appropriate discounted rate in order to calculate present value.
```

---

#### Discount Rate, Interest Rate, Cost of Capital
The terms can be used interchangeably.
- **Interest rate** is typically used for a debt security
- **Discount rate** or **cost of capital** is typically used for a project or a business

#### Compound Interest, Simple Interest
Investors benefitted from interest on interest.

Depending on simple or compound interests, the formula to calculate present/future value differ.

For **simple interest**, 

$$FV\ in \ n \ periods=PV(1+r\cdot n)$$

$$PV=\frac{FV\ in \ n \ periods}{(1+r\cdot n)}$$

---

#### Risk Adjustment

A discount rate should account for both the **time value of money** and the **risk** of achieving the **projected future cash flow**.

```ad-note
If the value of something is indiciated as "expected value", then treat it as the **future value**. **Current value** is sometimes expressed as **intrinsic value** or **fair value**.
```


#### Expected Return

$$\mathrm{Return}=\frac{\mathrm{Ending\ Value-Starting \ Value}}{\mathrm{Starting \ Value}}$$

Or, alternatively, if **periodic return** is given:

$$\mathrm{Return}=(1+\mathrm{Periodic \ Return})^n-1$$

---

#### Variable Interest Rates

Given a variable interest rate $r_t$ at time $t$, we have
$$FV=PV\cdot \prod_{t=1}^{n} \left(1+r_t\right)$$
