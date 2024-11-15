[[2024-11-12]] #Regression 

The point of our enterprise thus far has been to account for the **bias-variance tradeoff** when deciding how complex to make our model.
- Sometimes **increased complexity can lead to our prediction error increasing** when trying to use our data set to make predictions for new observations

Our solution so far - **incorporate complexity penalties** when comparing models of different size.

---
### Sample Splitting
We have briefly touched on this in [[17 Sample Splitting#Sample Splitting|lecture 17]]. Recall the definition of **Out-of-sample** $R^{2}$: ![[17 Sample Splitting#^e91f4c]]
```ad-example
**Example**: Extended Diabetes Regression Model

We re-run stepwise regression, but use only the **training data** to make the models. Contexts and results are as follows.

![[Pasted image 20241114165532.png|400]]
![[Pasted image 20241114165541.png|400]]

Naturally, the full model did best-in-sample. However, a simpler $5$-variable model does better OOS.
```

---
### Choosing Penalty
Our Information Theory Criterion were
- $AIC (\mathcal{M}_{m})=n \ln \left (\frac{RSS (\mathcal{M}_{m})}{n}\right)+2m$, and,
- $BIC(\mathcal{M}_{m})=n \ln \left(\frac{RSS(\mathcal{M}_{m})}{n}\right)+\ln(n)m$ 

So, they are generally of the form $$\text{Criterion}_{\lambda}(\mathcal{M}_{m})=n\ln \left (\frac{RSS (\mathcal{M}_{m})}{n}\right)+\lambda m$$ for difference choices of $\lambda$. This implies we could potentially choose $\lambda$ more intelligently!

In general, we aim to solve the following optimization problem to choose our model: $$\arg \min\limits_{\text{all models}} \text{Criterion}_{\lambda}(\mathcal{M}_{m})=n\ln \left (\frac{RSS (\mathcal{M}_{m})}{n}\right)+\lambda m$$ for $\lambda$. Intuitively, 
- Set $\lambda=0$ (no penalty) would result in the **full model** including all variables
	- Low bias, high variance (overly complex function, includes irrelevant predictors
- Set $\lambda \to \infty$ would return a model that **only includes an intercept**
	- Low variance, high bias 

Our goal is to optimize $\lambda$ to find the right trade-off between bias and variance.

```ad-attention
A seemingly innocuous approach, catered to forward stepwise, is grid search. For a grid of values of $\lambda$
- Run forward stepwise on the training set, and use $\text{Criterion}_\lambda$ as our complexity penalized metric
- Evaluate the performance for $\lambda$ on the test set
- Choose the $\lambda$ that maximizes performance on the test set 

However, the issue with this approach is that corrupts the nice properties of our out-of-sample $R^{2}$.
- When we didn’t optimize over $\lambda$, the $OSR^{2}$ truly **DID reflect a measure of performance on future observations**
- Optimizing over $\lambda$ is forming a **link** between training sets and test sets (has **ACCESS to** the test set in some sense).
	- The test set is meant for validation, but now it’s being used to **inform the prediction strategy**
	- Properties of the **test set** have **bled into** the choice of prediction function $\hat{f}$ - such that the two sets of observations are related
	- In other words, it *overfits* the test set - gives an unfair advantage in certain model comparisons
```

This leads to what is known as an **optimism bias**.
- The $OSR^{2}$ at the value of $\lambda$ that maximizes test set $OSR^{2}$ will be **larger**, on average, than what the $OSR^{2}$ would be when faced with future data

Many statistical methods have **tuning parameters**, or hyperparameters, associated with them. The example above reflects the issue at hand - we **CANNOT simultaneously**
1. Use the test set to choose the best value for them, and 
2. Use the test set to estimate out of sample performance of an algorithm given the optimized tuning parameters

We want to:
- Compare algorithms with different tuning parameters, and
- Compare an algorithm with a tuning parameter to one without

#### Validation Set 
A simple solution to the above would be to include a third split: a **validation** set.
- Partition data set three ways: training, validation, test
- $60-20-20$ might be reasonable

Consider, for a given algorithm,
- Use the training set to come up with a model, while **optimizing** tuning parameters based on predictive performance **on the validation set**
- Once the optimal tuning parameters have been chosen, **evaluate out-of-sample** predictive performance on the **test set**

Since the test set has been kept out of the selection process, this scheme does give trustworthy out-of-sample performance estimates and **removes optimism bias**.

While this would work, it may seem like we’re **sacrificing a lot of data** just for the sake of estimating a tuning parameter...
