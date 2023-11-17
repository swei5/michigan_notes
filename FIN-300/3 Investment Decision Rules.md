[[2023-01-30]] #Investment #CashFlow #NPV #DiscountRate 

### Ranking Projects
There are **two** types of investment decisions
1. **Accepting** or **rejecting** individual projects
2. **Selecting** the best projects from a pool of alternatives, that is, ranking projects

We need to decide whether investment decision rules allow us to effectively rank projects.

```ad-note
In an ideal world, we want to accept all positive NPV projects; however, in real life, project selection almost always comes down to ranking.
```

#### Criteria Set
In order to maximize equity value, we need to consider the following factors:
- Timing
- Magnitude of cash flows
- Risk

We will apply a set of decision rules to these projected cash flows to determine whether those rules help us in decision-making.

---

### Net Present Value
Recall how we compute NPV using annuity: ![[2.2 Discounted Cash Flow II#^28bede]]
The **net present value** is the **most important** decision rule in capital budgeting.
- Decision Rule: accept projects with the **highest** net present value

The net present value is the **important** decision rule in **capital budgeting**.

| Criteria              | Y/N               |
| --------------------- | ----------------- |
| Adjust for TVM?       | Y (Interest rate) |
| Adjust for magnitude? | Y                 |
| Adjust for risk?      | Y (Discount rate) |
| **Rank**                  | **Y**                  |

---

### Payback Period
The payback period rule is **biased** towards projects with **positive short-term cash flow**.
- Decision Rule: accept a project if the payback period is less than some pre-specified limit

```ad-example
![[Pasted image 20230130115923.png|600]]
```

| Criteria              | Y/N                                                |
| --------------------- | -------------------------------------------------- |
| Adjust for TVM?       | No. All cash flows pre payback are treated equally. |
| Adjust for magnitude? | Partially. Cash flows after payback are ignored    |
| Adjust for risk?      | No.                                               |
| **Rank**                  | **N**                                                  | 

---

### Discounted Payback Period
Compared to normal payback period, we discount the expected net cash flows to present value until NPV reaches 0.
- Decision Rule: accept a project if the discounted payback period is less than some pre- specified limit

| Criteria              | Y/N                                                |
| --------------------- | -------------------------------------------------- |
| Adjust for TVM?       | Partially. For pre-payback cash only. |
| Adjust for magnitude? | Partially. Cash flows after payback are ignored    |
| Adjust for risk?      | Partially.  For pre-payback cash only.                                               |
| **Rank**                  | **N**                                                  | 

The decision rule is still **biased** towards projects that generate **cash flows in the short-term**. We cannot compare projects with fundamentally different **timing** of cash flows.

```ad-note
It is still useful in quick assessment of project viability before committing resources to analyzing net present value.
```

---

### Internal Rate of Return

```ad-important
**Definition 3.1**: Internal Rate of Return (IRR)

The **internal rate of return** (IRR) is defined as the discount rate at which the **NPV** becomes zero:
$$
NPV_{r=IRR}\ \ \ \ s.t. \ \ \ \ C_0+\frac{C_1}{(1+IRR)}+\cdots+\frac{C_T}{(1+IRR)^T}=0
$$
```

^d8138a

- Decision Rule: accept the project if the **internal rate** of is the return exceeds the **discount rate** (that is, accept if the **internal rate of return** is **above a return** you consider fair given the project risk).

For this, we have to use **trial and error**
- In excel, we can use `=IRR(cashflows: 1d-array)`.
	- In this excel formulae, the first cash flow occurs at **time** **zero**, meaning that the input should include **ALL** cashflows.

| Criteria              | Y/N                                                |
| --------------------- | -------------------------------------------------- |
| Adjust for TVM?       | Y |
| Adjust for magnitude? | Y    |
| Adjust for risk?      | Y                                               |
| **Rank**                  | **N**                                                  | 

The rule adjusts for both TVM, magnitude, and risk, but **CAN'T** used to rank project.
- We cannot compare a **large** project to a **small** project
	- Since IRR is expressed as a **percentage**, it can make **small** projects appear more **attractive** than large ones.
- We cannot compare projects that have **fundamentally** **different** lives.
	- Suppose we had a 5 year project with IRR of 12% versus a 10 year project with IRR of 10%, we would have chosen the 5 year project. But what if we were not able to find another project that generates 12% IRR after the first 5 years? 
    
```ad-note
It is, however, very useful for presentation purposes. Net present value comparisons can be hard to **benchmark**. Is the net present value high or low? But the internal rate of return can be compared to returns earned on **past investment**s, and the returns currently on offer on corporate bonds.
- For example, historical average equity market return is about 8% per year in real terms
	- An internal rate of return on a project of **more than 10%** is high, all else equal
```

---

### Profitability Index

```ad-important
**Definition 3.2**: Profitability Index

Profitability Index is defined as the **present value** of **future cash** flows divided by the **initial investment**:
$$PI=\frac{NPV}{\mathrm{Initial\ Investment}}$$
```
- Decision Rule: Accept project if profitability index > 1

```ad-example
![[Pasted image 20230218212900.png|600]]
```

```ad-summary
The three propositions below are equivalent
- PI $>1$
- NPV is positive
- IRR $>$ Discount Rate
```

Thus, PI doesn't really tell us anything from that NPV tells us.

However, we cannot rank projects by the PI because the ranking will be affected by
- How you define the **investment** versus **operational** cash flows
- Scale of the projects

---

### NPV & IRR:  Issues
Project ranking can shift, contingent upon the **discount rate** and the **timing** of cash flows.
- At **lower** discount rate, **growing** cash flow project creates more value
- At **higher** discount rate, **constant** cash flow project creates more value

When the cash flows of a project change sign more than once, we may have **multiple** IRRs or no IRR at all.
- To solve this, we  could set a **different discount rate** to the cash flows that reflect the activities causing the negative cash flow (like clean-up costs).

```ad-warning
Different parts (phases) of a project are exposed to **different** **risks** and therefore should be discounted at **different** **discount** **rates**.
```

---

### Summary
![[Pasted image 20230218215635.png|700]]
