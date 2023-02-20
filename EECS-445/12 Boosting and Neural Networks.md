[[2023-02-15]] #Boosting #DecisionTree #NeuralNetwork


### Recap, The Boosting Property
Recall the AdaBoost algorithm: ![[11 Ensembles (Boosting)#^367ada]]

The Boosting property is as below:
$$\sum\limits_{i=1}^{n}\tilde{w}_m^{i} [[h(\overline{x};\overline{\theta}_{m})\ne y^i]]=0.5$$
```ad-important
**Intuition**: Since the current classifier achieves weighted error smaller than 0.5, there is more weight on correct examples. We increase weights on incorrect examples and decrease weights on correct examples so that they are perfectly balanced.
```

