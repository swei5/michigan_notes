[[2024-03-12]] #Accounting #Valuation 

### Motivation 
We know that firm value is determined by **cash flows** to firm and the **risk of assets**, and we're interested in how debt-equity mix affects $\text{UFCF}$, $r$, and $\text{EV}$.
- In its utmost simplified form, $\text{EV}=\frac{\text{UFCF}}{r}$, if viewed as a perpetuity and no CapEX

In this chapter, we are going to look at how **changes in capital structure** affect the firm value, all else being equal. Capital restructuring involves changing the **leverage** $(D/E)$ **WITHOUT changing firm assets**.
- Increase leverage by **issuing debt and buying back shares**
- Decrease leverage by **issuing new shares and redeeming outstanding debt**

### Derivation 
Assume at the moment that
$$\text{EV}_{L}=\text{EV}_{U}+\tau_{C}\cdot D$$
And $$\text{EV}_{L}=D+E$$
Where $\text{EV}_{L}$ is the market value of a **levered firm**, and $\text{EV}_{U}$ is that of a **unlevered firm**, and $\tau_{C}$ being the corporate tax rate.

Hence we have $$D+E=\text{EV}_{U}+\tau_{C}\cdot D \to \text{EV}_{U}=E+(1-\tau_{C})D$$
We can thus define [[9 WACC#^335717|WACC]] for an unlevered firm as
$$r_U=\frac{E}{\text{EV}_{U}}r_E+\frac{D}{\text{EV}_{U}}r_{D}(1-\tau_{C})$$
Multiplying both sides by $\text{EV}_{U}$ gets us
$$\text{EV}_{U}r_{U}=r_{E}\cdot E+r_{D}\cdot D\cdot (1-\tau_{C})$$
Which says **cash flow available to an unlevered firm** is the **sum** of **dividends** and **after-tax interest payments** of that firm.