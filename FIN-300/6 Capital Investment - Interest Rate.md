[[2023-02-19]] #DiscountRate #Valuation #CashFlow #DiscountRate 

### Inflation, Nominal and Real Rates
- Nominal prices represent the **actual** **prices** paid in **transactions**.
- Real prices reflect the **purchasing** **power** of transactions relative to some starting point.

```ad-example
![[Pasted image 20230219224325.png|600]]
```

In terms of interest rates, 
- Nominal rate of interest: interest earned, **NOT** adjusted for inflation
- Real rate of interest: interest rate obtained after considering the impact of **inflation**

#### Fisher Equation
The Fisher equation defines the relationship between real rates, nominal rates, and inflation:
$$(1+r)\cdot(1+\pi)=1+i$$
$$\to r=\frac{1+i}{1+\pi}-1$$

where $r$ is real interest rate, $\pi$ is inflation, and $i$ is nominal interest rate.

If we expand the formulae above, since both $r$ and $\pi$ are small values, **real interest rate** plus **inflation** is approximately **nominal interest rate**.

---

### Application in Project Valuation
In the scenarios where inflations are accounted, we need to adjust our calculations to real terms.

1. Compiling income statement
	- Revenue, COGS and expenses need to be **adjusted** to nominal terms by the **inflation** factor
2. Estimating free cash flow
	- Asset sales and additional cash flows need to be **adjusted** to nominal terms by the **inflation** factor
	- Depreciation and amortization **NOT** adjusted
3. NPV, IRR, and pay back period
	- If we are given **real discount rate**
		- Need to convert to the **nominal discount rate** using the **Fisher Equation**
	- If we are to calculate IRR, we would need to convert the nominal IRR to **real IRR**, through the **Fisher Equation** too