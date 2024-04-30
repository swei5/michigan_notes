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

---
### Diluted Shares Outstanding
Shareholder exercise of **warrants/options** implies **NEW issuance** of shares.

```ad-example
**Example, Treasury Stock Method**

Assume that existing shares outstanding = 200m, deal price = $48, and there are 4m options with exercise price $30 that were issued five years ago.

The number of total diluted shares outstanding after the options are executed can be computed as the following:
- Total proceeds to firm from issuing 4m new shares = $30 * 4m = $120m
- Repurchase of new shares at the deal stock price = $120m / $48 = 2.5m
- Net new issuance = 4m - 2.5m = 1.5m
- Thus, total diluted shares outstanding = 200m + 1.5m = 201.5m
```

Note that this method assumes that the **proceeds a company receives** from an in-the-money option exercise are used to **repurchase** common shares in the market.

```ad-example
**Example, [Equity Kickers](https://www.investopedia.com/terms/k/kicker.asp#:~:text=of%20future%20profits.-,Equity%20Kickers,said%20to%20be%20a%20kicker.)**

Assume that a total of 260 million shares were issued post-LBO, accounting for financial sponsor as well as management rollover and new investment.  
- Equity kickers to mezzanine debt holders: 4% of full diluted shares outstanding post-LBO 
- Equity kickers to management: 3.2% of full diluted shares outstanding post-LBO

What is the full diluted shares outstanding post-LBO $N$?
- $260+4\% \cdot N + 3.2\% \cdot N = N$
```

---
### Pro Forma Balance Sheet
Make necessary adjustments to the balance sheet utilizing the information from the **sources and uses schedule**.
- **Retire existing debt and equity**
- **Introduce new debt instruments** according to the financing structure
- The overpayment of equity goes into addition to **goodwill**, which should be **amortized** over time or tested for impairment every year
- Adjustments for deal-related fees
	- Financing fees show up on the balance sheet
	- All non-financing fees are deducted from equity contribution, so are tender/call premium fees

---
### Debt Schedule 
There are two parts we need to consider:
1. Prepayment of debt instruments 
	- Calculate the **required cash increase**
	- Calculate the **mandatory pay-down of principal**
	- Apply the available cash flows for **voluntary pay-down** of debt against **most senior debt** instruments whenever possible
2. Make necessary adjustments to the three statements for financing fees
	- Update the **amortization of deferred financing fees in income statement** as part of calculation for **interest expenses**
	- Update the deferred financing fees in balance sheet
	- Update the amortization of financing fees in the **Operating Activities** of cash flow statement, which is another **add-back** like the amortization of intangibles