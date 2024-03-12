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
We know that the [[9 WACC#^335717|WACC]] for a levered firm is (1): $$\text{WACC}=\frac{E}{\text{EV}_{L}}r_E+\frac{D}{\text{EV}_{L}}r_{D}(1-\tau_{C})$$
And intuitively, the **return on equity of an unlevered firm**, or WACC of an unlevered firm is
$$r_U=\frac{E}{\text{EV}_{U}}r_E+\frac{D}{\text{EV}_{U}}r_{D}(1-\tau_{C})$$
Multiplying both sides by $\text{EV}_{U}$ gets us
$$\text{EV}_{U}r_{U}=r_{E}\cdot E+r_{D}\cdot D\cdot (1-\tau_{C})$$

```ad-note
This says **cash flow available to an unlevered firm** is the **sum** of **dividends** and **after-tax interest payments** of that firm. Cash flow is distributed among all providers of capital, both equity and debt holders.
```

From the formulae above, if we sub $\text{EV}_{U}$ with $E+(1-\tau_{C})D$, we would get $$E(r_{E}-r_{U})=D\cdot(r_{U}-r_{D})\cdot (1-\tau_{C})$$
Which states that **incremental dividends** is equal to **after-tax interest savings**.

Rearranging the above further gives (2):
$$r_{E}=r_{U}+\frac{D}{E}(r_{U}-r_{D})\cdot(1-\tau_{C})$$
Where $r_{E}$ is equivalent to **equity risk**, $r_{U}$ is equivalent to the **business risk**, and $D/E$ is equivalent to the **financial risk** faced by the company. 

```ad-note
In other words, equity holders demand **higher compensation** as the **debt-financing increases the financial risk**.
```

If $r_{D}=0$, meaning debt is **risk-free**, 
$$r_{E}=r_{U}\left(1+\frac{D}{E}(1-\tau_C)\right)$$

Subbing (2) into (1) gives (3):  $$\begin{align}\text{WACC}&=\frac{E}{\text{EV}_{L}}r_{U}+\frac{D}{\text{EV}_{L}}(1-\tau_{C})\\&=r_{U}\left(1-\frac{D}{\text{EV}_{L}}\tau_{C}\right) \end{align}$$ Multiplying $\text{EV}_{L}$ to both ends yields $$\text{WACC}\cdot \text{EV}_{L}=r_{U}\left(E+D(1-\tau_{C})\right)$$
Note that this is effectively $$\text{WACC}\cdot \text{EV}_{L}=r_{U}\cdot \text{EV}_{U}$$
Which translates to the **cash flows to a levered firm** is equal to that of **an unlevered firm**.