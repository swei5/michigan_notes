[[2024-02-08]] #Valuation #Options 

### Real options
A real option gives a firm's management the right, but not the obligation to undertake certain **business opportunities or investments**.
- Multi-stage investments 
- High dispersion of payoffs depending upon ultimate success or failure
- Drug development, device research, etc.

Pathways create options. Risk lowers value, and **options increase value**.

In fact, all investments can be valued using real options analysis. But the option value increases along with **multiple stages**, **dispersion of payoffs** and **flexibility** in decision-making.

There's some distinction between irreversible investment and investment with **options**.

```ad-example
**Irreversible Investment, Example** 

![[Pasted image 20240208164419.png|600]]
```

```ad-example
**Investment with Options, Example**

![[Pasted image 20240208165223.png|600]]
```

---
### Valuation
Our fundamental valuation principle has always been that the value of any asset is the **present value of expected cash flows**, or, the probability weighted average of all possible cash flows.
- However, for projects with options the use of a scenario based model fails, because there are many paths the project could take & a single scenario based model cannot handle this

Real options analysis can be implemented in two phases, with the first phase being most important.  
- **Phase 1**: use a **binomial tree** to correctly work out the timing & magnitude of **expected cash flows**, & convert them to present value using a risk-adjusted discount rate. This ensures that we correctly estimate the expected cash flows
- **Phase 2**: extend the analysis to incorporate the **risk reduction that comes from embedded options** 

The valuation performed at Phase 2 should **always be HIGHER** than the valuation performed in Phase 1 & is the more accurate valuation. Specifically ,
1. Value the project as if you were **fully committed** to seeing the project to completion by working backwards  
	-  Value today depends upon what something is expected to sell for later, and a discount to reflect risk and the time value of money  
	- *Why consider the fully committed project?* We want a framework for comparing projects with options, and without options, to see how much **extra value we get from flexibility** in decision-making  
2. Value the project as if you were **not fully committed** to seeing the project to completion by working backwards  
	- Higher expected cash flows as expected cash flows are the probability weighted average cash flows from the **possible outcomes**; the expected cash flows increase because you re-assess the project at each stage and **ONLY exercise valuable options**. 
3. Account for the lower risk of a project with options by using **[[1 Options, Binomial Tree Valuation#^d138f6|risk-neutral probabilities]]**
	- Same process used in binomial tree valuation: $p=\frac{\mathbb{E}(S_{T})-L}{H-L}$

```ad-note
A project with options, compared to the same project without options, has **higher expected cash flows** and **lower risk**. Step 2 accounts for the **cash flow increase** and step 3 accounts for the **risk adjustment**.
```

For both step 1 and step 2, we first computed the **expected payoffs** then discount them to present values. At each decision-node, take `MAX(0, PAYOFF)` for all possible options.

The value of the option is equal to the **difference between Phase-2 valuation** (options) and **Phase-1 valuation** (fully committed valuation).

```ad-example
Note that in the example below the valuation of the project with option is higher - this is inhrently due to the delayed/flexible investment schedule.
- With options to invest after seeing whether if the investment is successful or not, we eliminate loss associated with a failure

![[Pasted image 20240213164405.png|600]]
```


```ad-example
**Calculating Risk-Neutral Probabilities**
```

