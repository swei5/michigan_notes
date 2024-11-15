[[2024-11-14]] #Regression

### Cross-Validation
The goal of cross-validation is to test the model’s ability to predict new data that were not used in estimating it, in order to flag problems like overfitting and to give an insight on how the model will **generalize to an independent dataset**. However, we’d like to do this in a way that isn’t overly sacrificial of training data.

```ad-important
**Definition 21.1**: $k$-fold Cross-Validation

We first partition the data into a training set and test set.
1. Given the training set, divide it into $k$ pieces (folds)
2. For fold $j$, use the other $k-1$ folds to construct a model, and test that model on the held-out fold $j$ (validation set) for a candidate hyperparameter value
3. 
```