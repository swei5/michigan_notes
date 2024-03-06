[[2024-03-05]] #Valuation #Modeling

### Overview 
- Basic approaches to forecast cash flows – core inputs for EV
	- Unlevered free cash flows - discounted cash flow analysis  
	- Levered free cash flows - capital structure theory
- Pro forma financial statement modeling
- Leveraged buyout analysis (LBO)
- Mergers and acquisitions analysis

EBIT is used as a measure for **GAAP net income**, but EBITDA is occasionally used and useful in many scenarios too.
- Cashflow

---
### Enterprise Value

```ad-important
**Definition 1.1**: Enterprise Value 

Enterprise Value (EV) is the value of firm's **core business activities**.

$$\begin{align}\text{EV}&=\text{Operating Assets}\\&=\text{Common Equity}+\text{Preferred Stock}+\text{Total Debt}\\+&\text{Non-controlling Interest of Book Equity}-\text{Cash}\end{align}$$
```

Cash is **NOT** part of EV because it doesn't reflect **productive activities**.

```ad-note
Given majority ownership $\alpha \in (0.5,1]$ on another entity whose financials have to be 100% **consolidated** onto parent firm’s financial statements, $1-\alpha$ is non-controlling interest.

To tackle this, **non-controlling interest** (aka minority interest) is added to EV so as to partially address inflated denominators in deal multiples such as EV/Sales or EV/EBITDA.
```

```ad-important
**Definition 1.2**: DCF Approach 
EV is present value of **future free cash flows** (FCF) discounted at the firm **WACC**. Unlevered FCF is defined as
$$\begin{align}\text{UFCF}&=\text{EBIT} - \text{Taxes} + \text{D\&A} - \Delta \text{NWC}-\text{Capex}\\&=\text{EBITDA} - \text{Taxes} - \Delta \text{NWC}-\text{Capex}\end{align}$$

Often times, we forecast FCFs over the next five years and estimate a terminal value:
$$\text{EV} = \sum_{t} \frac{\text{FCF}_{t}}{(1+\text{WACC})^{t}}+\frac{\text{Terminal Value}}{(1+\text{WACC})^{T}}$$

Where
$$\text{Terminal Value}=\frac{\text{FCF}_{T}}{\text{WACC}-g}$$

In practice, one approach is to use the **estimated figures on the Statement of Cash Flows**; or, to use the **pro forma financial statements** with a forecasted growth rate of sales and other figures scaled by sales.
```

**Unlevered FCFs** are essentially cash flows from **operating activities and investing activities** (only those related to **PRODUCTIVE** activities excluding investments on financial assets), but financing activities are excluded.

**Levered FCFs** will include **financing activities** which reflect choice of debt structures.