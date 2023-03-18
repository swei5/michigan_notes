[[2023-03-17]] #Stocks #Valuation #DiscountRate #Dividend 

### Stock Valuation
There are two types of values for stocks.

```ad-important
**Definition 10.1**: Market Value and Intrinsic Value

The **market value** of an asset – shares, firm, debt, project – is the **transaction** **price** between a buyer and seller who are well informed and willing to trade.
- Price is the market value

The **intrinsic value** of an asset is an estimate of what someone thinks is fair based upon an estimate of reasonable assumptions.
```

There are two primary ways of valuation:
- Discounted cash flow valuation
- Multiples valuation

Both techniques would allow us to estimate both the **intrinsic** and **market** value.

#### Discounted Cash Flow Valuation
Recall we have used this technique in almost evaluating **ANY** types of assets; bonds, for example: ![[7 Bonds I#^a866bd]]
uses a DCF model.

For stocks, there are two ways using DCFs:
1. Estimate the enterprise value (entirety) of the firm: projecting the **free cash flow**, discounted at a rate, and **subtracting** the value of **debt**
2. Dividend discount model: estimate **dividends per share** and discount at a rate (the cost of equity)

```ad-note
The choice as to whether to use the free cash flow to the firm approach or the dividend discount model depends upon the **information** **available** for analysis.

Note that for either approach, the final valuation should be **exactly the same**.
```

##### Free Cash Flow
See the process below.

```ad-important
**Definition 10.2**: Free cash flow valuation model

First, we calculate the value of **operating assets**:
$$\text{Value of Operating Assets}=PV_{CF, \text{expected}}=PV_{CF,\text{explicit forecast period}}+PV_{\text{terminal value}}$$

from which we obtain the **enterprise value**
$$\text{Enterprise Value}=OA+\text{Non-operating Assets including Excess Cash}$$

Then, we get the **value of equit**y as well as **equity value per share**:
$$\text{Value of Equity}=\text{Enterprise}-\text{Market Value of Debt}$$
$$EVPS=\frac{\text{Equity}}{N}$$

where $N$ is the number of **shares outstanding**.
```

```ad-important
**Definition 10.3**: Excess Cash
Excess cash is cash that is **NOT** needed as **working capital** to generate the forecast cash flows. A company can **immediately** use the cash to
- Pay dividends
- Buy-back shares
- Repurchase shares

There is **NO** impact on the projected cash flows.
```

One trick to estimate **cash required to operate** the business is
- Differences between revenue and EBITDA (1/12 to 3/12 of annual **cash operating costs**)
	- However, this always varies by industry and firm

---

##### Dividend and Discount
Similar to free cash flow model, this technique projects dividends per share for **ALL future** periods, which might include
- An explicit forecast period
- A terminal growth period

```ad-important
**Definition 10.4**: Dividend discount model

In this model, we compute the **present value** of **dividends per share** using a **discount rate** that reflects the **risk to equity** (called “the cost of equity capital”)
- The cost of equity will be **higher** than the discount rate related to the **whole firm**
	- Equity is **riskier** than the whole business because of residual claimant

$$EVPS=PV_{\text{explicit dividends},0\to i}+PV_{\text{forecasted dividends},i\to \infty}$$
```

---

#### Valuation Multiples
See below.

```ad-important
**Definition 10.5**: Valuation multiples:

There are six steps, in total:
1. Compile value multiples from **comparable firms**
	- Enterprise Value/Sales
	- Enterprise Value/EBITDA
	- Enterprise Value/EBIT
	- EPS/PPS
2. Median multiples
	- Median of the measure of all firms
1. Compile financial information for the firm to be valued
2. Estimate **enterprise value**, **equity value** and equity value per share from all valuation multiples
3. Summarize valuation estimates
4. Reach a conclusion
```

