[[2024-10-31]] #Regression 

### Bias-Variance Tradeoff, Revisited
Following our discussion from last lecture: ![[16 Overfitting#^16b2f2]]
As the **sample size** **increases**, the bias of an estimator and the variance of an estimator behave **differently**. For a fixed degree of complexity:
- Bias **DOES NOT** change much as $n$ increases.
	- Since bias has to do with **misspecification** of the true function
- Variance of an estimator **does** **DECREASE** as $n$ increases
	- Analogy: $\text{Var}(\bar{y})=\frac{\sigma^{2}}{n}$
	- The rate at which an algorithm’s variance decreases can vary, but typically any reasonable prediction function’s **variance will tend to zero**

Recall the decomposition of prediction error ![[16 Overfitting#^d960c7]]
```ad-note
Irreducible Error $\mathbb{E}[(y^{\star}-{f}(\tilde{x}))^{2}]$ **DOES NOT** change as $n$ increases. 

For a **fixed degree** of complexity, Squared Bias $(\mathbb{E}[\hat{f}(\tilde{x})]-f(\tilde{x}))^{2}$ **DOES NOT** predictably decrease or increase.

For a fixed degree of complexity, Variance $\mathbb{E}[(\hat{f}(\tilde{x})-\mathbb{E}[\hat{f}(\tilde{x})])^{2}]$ **decreases**. 

Hence for sufficiently large sample sizes, the reducible error is **dominated** by the **bias**.
```

The tradeoff shows us that we must avoid overfitting to data and the deficiencies in $R^{2}$ as a metric for model performance.
- **DOES NOT** account for model **complexity** at all
- Only compares reduction in error **IN SAMPLE**

Can we develop strategies for **choosing between models of different complexity** that reflect performance on future observations?
- We’ve actually seen one such strategy already under the assumption of the stronger linear model: partial- $\mathcal{F}$ test

We’ll now describe a general strategy for comparing models.

---
### Sample Splitting
Imagine taking our data set with n observations, and randomly splitting it into two **mutually exclusive**/disjoint and exhaustive sets.
- Training set of size $n_{\text{train}}$
- Test set of size $n_{\text{test}}$
- Typically, $n_{\text{train}} > n_{\text{test}}$
- For prediction task, a $70$-$30$ split might be reasonable

After creating this partition
1. Build a predictive model using **only the training set**
2. Evaluate its performance by how well it can predict the observations in the **test set**

```ad-note
In this set-up, it is essential that the test set **CANNOT** play any role in building the prediction algorithms.
- Needs to truly act as though it was new, as of yet unseen, data
- Prediction models fit in the training set need to be **INDEPENDENT** of the observations in the test set
```

In addition, future observations must be drawn from the **same distribution** as present observations for the held out test set to reflect performance on future observation. This is known as **generalizability** of a model
- In words, the present must be **representative** of the future

```ad-note
We refer to performance metrics defined using the **training set** as **in-sample**.

We refer to performance metrics defined using the **testing set** as **out-of-sample**.
```

Suppose, based on **only** on our **training** set, we fit a predictive model $\hat{f}(\cdot)$. For each of the $n_{\text{test}}$ individuals in our test set, we have responses $y_{i}$, along with the covariates $x$. We can generate predicted values for the test data based upon the model fit to the training set, $\hat{y}_{1},\cdots, \hat{y}_{n}$.

Then, we can define Out-of-Sample RMSE as follows:

```ad-important
**Definition 17.1**: Out-of-Sample RMSE

$$OS-RMSE=\sqrt{\frac{1}{n_{\text{test}}}\sum\limits_{i=1}^{n_{\text{test}}}(y_{i}-\hat{y}_{i})^{2}}$$
```

This is an **estimate of prediction error**, since none of the responses in the test set were used to form the predictions.

![[Pasted image 20241102163321.png|500]]

We see that as complexity increases, In-Sample RMSE becomes very low and Out-Sample RMSE exhibits bias-variance tradeoff.

We could similarly define a better $R^{2}$ using our training data to predict our test data. Recall that we motivated $R^{2}$ as a metric for **improvement relative to a baseline model** that only predicted the mean for all individuals.

Based on our training set, our best guess for the population mean is $\bar{y}_\text{train}$, which is what we’d predict for everyone in the test set under this simple model. And, by exploiting information in $x$ we predict $\hat{y}_{i}=\hat{f}(x_{i})$ for individual $i$ in the test set.

```ad-important
**Definition 17.2**: Out-of-Sample $R^{2}$

$$OSR^{2}=1-\frac{\sum_{i=1}^{n_{\text{test}}}(y_{i}-\hat{y}_{i})^{2}}{\sum_{i=1}^{n_{\text{test}}}(y_{i}-\bar{y}_{\text{train}})^{2}}$$
```

---
### Polynomials in Several Predictors
We can define polynomials with **multiple predictor variables**. For $p$ even moderate, the number of polynomial terms **explodes quickly** when doing a third degree or higher polynomial. In practice, people rarely go beyond 2nd degree polynomial with multiple predictors.

```ad-example
**Example**: $p=3$, second degree polynomials

Second degree polynomial: linear terms, quadratic terms, and all **pairwise interaction** terms between variables.

$$f(x)=\beta_{0}+\beta_{1}x_{1}+\beta_{2}x_{2}+\beta_{3}x_{3}+\beta_{4}x_{1}^{2}+\beta_{5}x_{2}^{2}+\beta_{6}x_{3}^{2}+\beta_{7}x_{1}x_{2}+\beta_{8}x_{1}x_{3}+\beta_{9}x_{2}x_{3}$$
```

Below is an example of running second-degree polynomials regression with $p=2$.

```r
> multpoly = lm(fev~polym(age, height, degree = 2, raw = T), data = dat)
> names(multpoly$coef)
[1] "(Intercept)" # 0.0
[2] "polym(age, height, degree = 2, raw = T)1.0"
[3] "polym(age, height, degree = 2, raw = T)2.0"
[4] "polym(age, height, degree = 2, raw = T)0.1"
[5] "polym(age, height, degree = 2, raw = T)1.1"
[6] "polym(age, height, degree = 2, raw = T)0.2"
```
