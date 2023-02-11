[[2023-02-09]] #DecisionTree #Ensemble #Bias #Variance 

### Information Gain
Recall the definition of **conditional entropy** ![[9 Decision Tree#^5c5641]] 
Information gain is defined as
$$IG(X,Y)=H(Y)-H(Y|X)$$

In other words, this denotes how much does **knowing** the value of $X$ **reduce** my **uncertainty** in the variable $Y$.

---

### Training Decision Trees: Algorithm
1. Start with an empty tree
2. Split on the best feature
3. Recurse until a **stopping criterion** is met

#### Stopping Criteria
1. When all data have the **same** **label** (assuming this is possible)
2. If all data have **identical** **features** (**NO** further splits possible)
3. If all features have **zero** IG