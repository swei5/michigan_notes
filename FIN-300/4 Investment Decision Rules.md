[[2023-01-30]] #Investment #CashFlow #NPV #IRR

### Ranking Projects
There are **two** types of investment decisions
1. **Accepting** or **rejecting** individual projects
2. **Selecting** the best projects from a pool of alternatives, that is, ranking projects

We need to decide whether investment decision rules allow us to effectively rank projects.

#### Criteria Set
In order to maximize equity value, we need to consider the following factors:
- Timing
- Magnitude of cash flows
- Risk

---

### Net Present Value
The **net present value** is the **most important** decision rule in capital budgeting.
- Accept projects with the **highest** net present value

---

### Payback Period
The payback period rule is **biased** towards projects with **positive short-term cash flow**.
- Example: 
	![[Pasted image 20230130115923.png|600]]

---

### Discounted Payback Period
he decision rule is still **biased** towards projects that generate **cash flows in the short-term**. We cannot compare projects with fundamentally different **timing** of cash flows.

---

### Internal Rate of Return
The internal rate of return (IRR) is defined as the discount rate at which the **NPV** becomes zero:
$$
C_0+\frac{C_1}{(1+IRR)}+\cdots+\frac{C_T}{(1+IRR)^T}=0
$$

You accept the project if the internal rate of is the return exceeds the discount rate (that is, accept if the **internal rate of return** is **above a return** you consider fair given the project risk).
- For this, we have to use **trial and error**

In excel, we can use `=IRR(cashflows)`.

The rule adjusts for both TVM, magnitude, and risk, but **CAN'T** used to rank project.
- We cannot compare a **large** project to a **small** project (since IRR is expressed as a **percentage**, it can make small projects appear more attractive than large ones).
- We cannot compare projects that have **fundamentally** **different** lives.

It is, however, very useful for presentation purposes. Net present value comparisons can be hard to **benchmark**. Is the net present value high or low? But the internal rate of return can be compared to returns earned on **past investment**s, and the returns currently on offer on corporate bonds.

---

### Profitability Index
**Present value of future cash flows** divided by the initial investment.