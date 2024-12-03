[[2024-11-26]] #Regression 

### Model Selection
Like in linear regression, we may more generally be interested in testing the null that a **certain subset of predictor variables** $\mathcal{I} \subset \{1,\cdots,p\}$   have slope coefficients equal to 0: $$\begin{align}
H_{0}&: \beta_1=\dots=\beta_{p}=0\\
H_{a}&:  \text{ at least one } \beta_{j}\ne0
\end{align}$$
In general, the resulting tests in logistic regression are the analogues of [[6 Categorical Variables#Partial $F$ -test|Partial F-tests]]. 

Like in linear regression, we will compare the in-sample performance of
- **Full Model**: Logistic Regression fit using all predictor variables
- **Reduced Model**: Logistic Regression fit only on the predictor variables whose slopes aren’t equal to zero under the null

In the case of logistic regression, we compare
- Likelihood of observed data under the full model
- Likelihood of the observed data under the reduced model

Where likelihood is defined as $$L(\tilde{\beta}|y,x)=\prod_{i=1}^{n}(p_{\tilde{\beta}}(x_{i}))^{y_{i}}(1-p_{\tilde{\beta}}(x_{i}))^{1-y_{i}}$$
The likelihood under the full model **will NEVER be smaller** (just like RSS was never larger in the full model). We account for this in hypothesis testing.

#### Generalized Likelihood Ratio Test
Our test statistic will be based upon a comparison of the log-likelihoods. Let $\hat{\beta}_{\text{full}}$ and $\hat{\beta}_{\text{red}}$ be the estimated coefficients in the full and reduced morels; $L(\hat{\beta}_{\text{full}}|y,x)$ and $L(\hat{\beta}_{\text{red}}|y,x)$ be the likelihoods evaluated at these values for $\hat{\beta}$; $p_{\text{full}}$ and $p_{\text{red}}$ be the number of predictor variables in the full and reduced models.

We consider the following as a test statistic $$-2[\log(L(\hat{\beta}_{\text{red}}|y,x))-\log(L(\hat{\beta}_{\text{full}}|y,x))]$$ Under the null hypothesis, as $n \to \infty$, $$-2[\log(L(\hat{\beta}_{\text{red}}|y,x))-\log(L(\hat{\beta}_{\text{full}}|y,x))] \xrightarrow{d} \chi^2_{(p_{\text{full}}-p_{\text{red}})}$$
Note that $-\log(\cdot)$ is the log-loss and $-2 \cdot \log(\cdot)$ is called the **deviance**.

The general framework for model selection is nearly identical to that in linear regression, so we’ll now describe it briefly. For logistic regression, the general approach is to replace residual sum of squares by **two times log-loss** (the loss function used for logistic regression): $$\min_{\hat{\beta}} 2\sum\limits_{i=0}^{n}(-y_{i}\log(p_{\tilde{\beta}}(x_{i}))-(1-y_{i})\log(1-p_{\tilde{\beta}}(x_{i})))+\lambda \sum\limits_{j=1}^{p} \mathbb{1}(\tilde{\beta}_{j}\ne 0)$$
When 
- $\lambda=2$: AIC
- $\lambda=\log(n)$: BIC
- Even better, we can choose $\lambda$ through cross-validation

However, as with best subsets in linear regression, model selection for logistic regression suffers from computational hurdles.
- Can consider [[19 Model Selection I#^4ee7e8|forward stepwise/backwards]] elimination - the `step` function in `R` works for logistic regression
- Consider instead [[22 Regularization|regularization]]: $$\min_{\hat{\beta}} 2\sum\limits_{i=0}^{n}(-y_{i}\log(p_{\tilde{\beta}}(x_{i}))-(1-y_{i})\log(1-p_{\tilde{\beta}}(x_{i})))+\lambda \sum\limits_{j=1}^{p} |\beta_{j}|^{q}$$
This is implemented in `cv.glmnet` as well by adding argument `family="binomial"`.

A straightforward out-of-sample metric is the **OOS Log Loss**. For any candidate model, take the corresponding coefficient estimates $\hat{\beta}$. Calculate in the test set $$\text{LogLoss}_{OOS}=\sum\limits_{i=1}^{n_{\text{test}}}(-y_{i}\log(p_{\tilde{\beta}}(x_{i}))-(1-y_{i})\log(1-p_{\tilde{\beta}}(x_{i})))$$
This metric isn’t all that common in practice.

---
### Regression and Classification
Trying to predict future values of a **continuous response** using covariate information is typically called a **regression** task.
- Given $x$, our prediction is directly given to us by $\hat{f}(x)$

Trying to predict future values of a **categorical response** using covariate information is typically referred to as **classification**.
- Given $x$, we can form an estimate for $\hat{p}(x)$ using logistic regression - but we don't want to use $\hat{p}(x)$ itself as our prediction since future response is either 0 or 1!

When using logistic regression for classification, there’s an additional step to go from our estimate $\hat{p}(x)$ to our eventual category.
- Numerical values for our estimated function need to be **mapped** to categories
- The mapping depends on the **particular classification task** - what are the costs of various mistakes?

For classification tasks, other metrics are common in practice:
- Accuracy
- True positive rate (TPR) and false positive rate (FPR)
- Area under the curve (AUC)
- Average cost based on a cost matrix

---
### Course Wrap-up
We began by studying theoretical and applied aspects of **ordinary least squares** regression under the assumptions of the weaker or stronger linear models.
- [[2 Multiple Regression|Where do the coefficients come from?]]
- [[3 OLS Estimators|What are the distributional properties of the coefficients?]]
- [[5 Confidence Intervals|How can I perform inference for the coefficients?]]
- [[8 Prediction Intervals|How can I form prediction intervals for future observations?]]

We then thought about ways to detect **whether the underlying assumptions were reasonable**, and common practical concerns when running multiple regression and interpreting the results.
- [[9 LR Assumptions|Diagnostic plots]]
- [[10 Outliers|Leverage and Outliers]]
- [[11 Collinearity|Multicollinearity]]
- [[12 Multi Hypothesis Tests|Multiple Comparisons]]

We next considered how to proceed with inference when **homoskedasticity and/or normality** are violated.
- [[13 Heteroskedasticity|Weighted least squares / HC standard errors (heteroskedasticity)]]
- [[14 Non-Normality|Bootstrapping (non-normality)]]

We further considered approaches for using linear regression to **fit nonlinear functions**, and fundamental considerations to keep in mind when fitting flexible functions.
- [[16 Overfitting|Polynomial Regression]]
- [[18 Polynomials Fitting|Regression and Natural Splines]]
- [[18 Polynomials Fitting|Generalized Additive Models]]
- [[17 Sample Splitting|Bias/Variance Tradeoff]]
- [[17 Sample Splitting|In-Sample vs Out-of-Sample Performance]]

We finally discussed how intuition from fitting flexible nonlinear functions helps us consider the important topics of model building and a**voiding overfitting in multiple regression**.
- [[19 Model Selection I|Penalizing Model Complexity]]
- [[19 Model Selection I|Model Selection through p-values]]
- [[19 Model Selection I|Best Subset Selection]]
- [[19 Model Selection I|Forward Stepwise/Backwards Elimination using Criterion]]
- [[21 Model Selection III|Cross-Validation to choose the right penalization]]
- [[22 Regularization|Regularized Regression (LASSO and Ridge)]]

We then wrapped up by discussing logistic regression. Used when outcomes are binary, but there are **natural analogues** between our discussion of linear regression and what occurs with logistic regression.
