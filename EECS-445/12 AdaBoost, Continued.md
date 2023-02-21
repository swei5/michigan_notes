[[2023-02-15]] #Boosting #DecisionTree #Ensemble 


### Recap, The Boosting Property
Recall the AdaBoost algorithm: ![[11 Ensembles (Boosting)#^367ada]]

```ad-important
**Theorem 12.1**: The Boosting Property:

$$\sum\limits_{i=1}^{n}\tilde{w}_m^{i} [[h(\overline{x};\overline{\theta}_{m})\ne y^i]]=0.5$$
```

```ad-note
**Intuition**: Since the current classifier achieves weighted error smaller than 0.5, there is more weight on correct examples. We increase weights on incorrect examples and decrease weights on correct examples so that they are perfectly balanced.
```

---

### AdaBoost, Final Expression
Recall that AdaBoost aims to minimize the loss function in form of
$$J(\alpha_m,\overline{\theta}_m)=\sum\limits_{i=1}^{n}\exp(-y^{i}h_{M}(\overline{x}^i))$$
$$=\sum\limits_{i=1}^{n}\exp(-y^{i}(\alpha_{m}h(\overline{x}^i;\overline{\theta}_{m})+h_{m-1}(\overline{x}^i)))$$
Note that $h_{m-1}(\overline{x}^{i})=\sum\limits_{j=1}^{M-1}\alpha_{j}h(\overline{x}^i;\overline{\theta}_j)$.

$$=\sum\limits_{i=1}^{n}\exp(-y^{i}(\alpha_{m}h(\overline{x}^i;\overline{\theta}_{m}))\exp(-y^ih_{m-1}(\overline{x}^i)))$$
$$=\sum\limits_{i=1}^{n}\exp(-y^{i}(\alpha_{m}h(\overline{x}^i;\overline{\theta}_{m}))w^{i}_{m-1}$$
since ![[11 Ensembles (Boosting)#^240105]]
![[Pasted image 20230220005549.png|550]]
Hence, our final expression of our objective function is
$$J(\alpha_m,\overline{\theta}_m)=\exp(-\alpha_m)+(\exp(\alpha_m)-\exp(-\alpha_{m})) \sum\limits_{i=1}^{n}\tilde{w}_{m-1} [[h(\overline{x};\overline{\theta}_{m})\ne y^i]]$$

Thus, 
$$\overline{\theta}_{m}^{\star}=\mathrm{argmin}_{\theta} J(\alpha_m,\overline{\theta}_m)=\mathrm{argmin}_{\theta} \sum\limits_{i=1}^{n}\tilde{w}_{m-1} [[h(\overline{x};\overline{\theta}_{m})\ne y^i]]$$
Similarly,
$$\alpha_{m}^{\star}=\mathrm{argmin}_{\alpha} J(\alpha_m,\overline{\theta}_m)=\mathrm{argmin}_{\alpha}\exp(-\alpha_m)+(\exp(\alpha_m)-\exp(-\alpha_{m}))+\hat{\epsilon}_{m}$$
$$\alpha_{m}^{\star}=\frac{1}{2}\ln\left(\frac{1-\hat{\epsilon}}{\hat{\epsilon}}\right).$$

