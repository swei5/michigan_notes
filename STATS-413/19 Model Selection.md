[[2024-11-07]] #Regression 

Through our discussion of nonlinear regression (Lec 15-19), we understand the importance of choosing the right **level of complexity**.
- [[16 Overfitting|Bias-Variance Trade-Off]]

In linear regression, complexity rises with the **number of predictor variables** that we choose to include in our model.
- Want to include any predictor variable that’s truly meaningful for predicting the response (otherwise there would be **bias**)
- Want to ignore irrelevant predictors (these would just add noise to our predictions, increasing **variance**)

Imagine that we have $p$ covariates, and that linearity holds:  $$\mathbb{E}(y|x)=x^T \beta$$ Of the covariates $x_{1},\cdots, x_{p}$, we imagine that only some of them have nonzero slope coefficients.
- If $\beta_{j}\ne 0$, we’d like to use that variable for prediction

Our objective is to find the subset $\mathcal{M}_{m}\subseteq \{1,\cdots, p\}$ of $m \le p$ indices for covariates with nonzero slope coefficients.

There are $2^{p}$ possible models, each corresponds to a distinct subset $\mathcal{M}$, as for each variable, we may include or not include.