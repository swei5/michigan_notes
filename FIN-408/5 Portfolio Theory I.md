[[2024-01-30]] #Portfolio #Investment 

### Motivation 
The investment problem can be decomposed into two steps:
1. Find the optimal portfolio of risky securities, and
2. Find the best combination of the risk-free and this optimal risky portfolio 

---
### Portfolio 
A portfolio is a weighted collection of assets. Recall the **expected return** and **variance of return** of a portfolio: ![[8.1 Portfolio Theory#^b85381]]

```ad-note
Note that if our portfolio has exactly one risky assets and one risk-free asset, the variance of the portfolio is
$$\text{Var}(\mathbf{R}_{p})=w_{E}^{2}\sigma_{E}^{2} $$

As the return of a risk-free asset is **constant**.
```

#### Portfolio of $n$ Risky Assets 
Assume that $w_{i}=\frac{1}{n}$ (equally weighted). Then,
$$\text{Var}(\mathbf{R}_{p})=\left(\frac{1}{n}\right) \overline{\sigma_{i}}^{2}+\left(1- \frac{1}{n}\right)\overline{\sigma}_{ij} \to \overline{\sigma}_{ij}$$

In large portfolios, stocks’ own variances **cancel out** (diversified away) and the total portfolio risk reduces towards the average of covariances of all the assets in the portfolio. This **average covariance** is called the portfolio’s **systematic** (non-diversifiable) risk.

We will see later that, in asset pricing models, **systematic risk** is the **only priced risk**, i.e., the only risk that generates a compensation in terms of expected return.

![[Pasted image 20240201135458.png|500]]

Recall the definition of **systematic** and **unsystematic risk**: ![[8.2 CAPM#^4b21b3]]
```ad-summary
As the number of securities in a portfolio increases, the contribution of the **unsystematic** or diversifiable risk to the standard deviation of the portfolio **declines**.

Systematic or non-diversifiable risk is **NOT reduced** even as we increase the number of stocks in the portfolio
```

#### Relationship between Expected Return and Risk 
Given by
$$\mathbb{E}(\mathbf{R}_{p})=r_{f}+\frac{\mathbb{E}(r_{E})-r_{f}}{\sigma_{E}}\sigma_{p}$$
Given a risky asset $f$ and risky equity $E$ in a portfolio $p$.  Here, $\frac{\mathbb{E}(r_{E})-r_{f}}{\sigma_{E}}$ is the **sharpe ratio** of the risky asset $E$ and is the slope of the **capital allocation line** (CAL).

```ad-example
Portfolio of two risky assets ($\rho = 1$)

![[Pasted image 20240201141249.png|500]]
```

```ad-example
Portfolio of two risky assets ($\rho = 0$)

![[Pasted image 20240201141329.png|500]]
```

```ad-example
Portfolio of two risky assets ($\rho = -1$)

![[Pasted image 20240201141726.png|500]]
```

The relationship between risk and expected return of the portfolio is **NOT LINEAR** and it also depends on the **correlation between the two risky assets**.