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
	- Might be short-sighted

#### Pseudo Code
```python
BuildTree(DS)  
	if (y(i) == y) for all examples in DS
		return y  
	elif (x(i) == x) for all examples in DS
		return majority label
	
	else
		xs = argminx H(y|x) 
		for each value v of x_s
			DS_v = {examples in DS where x_s = v} 
			BuildTree(DSv)
```

**Stopping criteria #1**
```python
if (y(i) == y) for all examples in DS
	return y
```

**Stopping criteria #2 **
```python
elif (x(i) == x) for all examples in DS
		return majority label
```

