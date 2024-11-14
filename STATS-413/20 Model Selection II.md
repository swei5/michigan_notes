[[2024-11-12]] #Regression 

The point of our enterprise thus far has been to account for the **bias-variance tradeoff** when deciding how complex to make our model.
- Sometimes **increased complexity can lead to our prediction error increasing** when trying to use our data set to make predictions for new observations

Our solution so far - **incorporate complexity penalties** when comparing models of different size.

---
### Sample Splitting
We have briefly touched on this in [[17 Sample Splitting#Sample Splitting|lecture 17]]. Recall the definition of **Out-of-sample** $R^{2}$: ![[17 Sample Splitting#^e91f4c]]
