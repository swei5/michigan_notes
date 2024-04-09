[[2024-04-02]] #Valuation #Accounting #Investment 

### Cash Flows, Revisited
In DCF analysis, FCF is cash flow from unlevered operating assets, aka **unlevered FCF**. ![[1 DCF#^cf1ae2]]
Whereas in LBO analysis, FCF is cash flow from operating assets **after making interest payments** on debt, aka **levered FCF** or “cash available for principal repayment”.
$$\text{LFCF}=\text{UFCF}-\text{Interest Expense After Tax}-\text{Mandatory Amortization}$$
Thus, debt balance at the end of period $i$ becomes
$$D_{i}=D_{i-1}-\text{LFCF}_{i}$$
Assuming all levered cash flow is used to payoff some of the debt balance. Most LBOs work on this principle – **pay down the debt as quickly** as possible.

Assume $\text{EV}$ stays constant throughout $T$ periods for the simplicity of model, then
$$E_{T}=\text{EV}_{0}-D_{T}$$
From which we can calculate the internal rate of return (IRR) of **equity** using the `IRR` function in excel.
- The higher the exit price (EV), the higher the IRR
- The higher the leverage (lower initial equity contribution), the higher the IRR
- The more debt paid down, the higher the IRR

We can use Excel `CHOOSE` function to model cash flow scenarios. Typically, cash flow and financing scenarios are modeled separately.
- `=CHOOSE(k, value1, ..., valuen)` chooses the ` k` th value

---
### LBO Complications 
There are two complications with LBO analysis:
- We need to **track paying down of debt** over time
	- Affects next period’s interest payments, which **affects next period’s levered FCF**
		- Which in turn, is used to pay down debt next period
	- Along the way, there may be **cash flows to equity** before exit
- We want to be able to consider the effects of different financing structures
	- There are many layers of financing for debt
	- Each layer has its own interest cost, and sometimes its own principal repayment schedule
	- Within each year, firm may or many not have flexibility over timing of debt payments

---
### Goodwill 
The LBO sponsor is buying the **whole firm** - it obtains all the equity and pays off all the current debt but usually pays **MORE THAN the book value** of equity. The difference between the amount offered to equity holders and the book value of equity is called **Goodwill**:
$$\text{Goodwill}=E_{\text{Offer}}-E_{\text{Pre-LBO}}$$