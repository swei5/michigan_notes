[[2024-10-29]] #Regression 

As we know by now, a key assumption underpinning multiple regression is the assumption of **linearity**. In practice, we might instead believe $$\mathbb{E}(y|x)=f(x_1,\dots,x_{p})$$ such that our real data are noisy observations from some complicated, **nonlinear**, **multivariate** function $f(\cdot)$. 

If there’s a particular transformation of $y$ or $x$ that is known/suspected to make relationship linear, apply it (e.g. log transformation).