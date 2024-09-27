[[2024-03-14]] #Derivatives #Valuation 

A forward contract is an agreement to transact in the future. It can be written to **meet particular requirements**, but buyers and sellers are responsible for **managing counterparty default risk**.

A futures contract is a **standardized forward contract** traded on an **exchange**. It offers **liquidity and protection** to buyers and sellers because a clearing house takes on default risk.
- Offset by a **requirement** of buyers and sellers to post a margin, e.g. 10% of payoff to close the position
- Can be traded on exchanges

In this section, we will treat all contracts – futures and forwards – as **forward contracts**. This means we will not consider the ongoing requirement to post a margin.

### Hedge Contracts 
The number of contracts to purchase can be expressed as $$n=\frac{V_{a}S_{a}\rho_{a,h}}{X_{h}}$$
Where $X$ is the strike price of forward contract, $V$ is volume, $S$ is the spot price, and $\rho$ is the regression coefficient between the % change in price of the asset and the % change in the price of the underlying asset of the forward contract.

```ad-example
![[Pasted image 20240425131256.png|400]]

The regression coefficient $\rho$ describes the correlation between $y$: the target asset class we are trying to **HEDGE**, and $x$: the **type of the underlying asset of the forward contract** we are buying.
```

This would create what's known as a **perfect hedge**. This is the case because we account for both the size of the forward contract (how many contracts we need to purchase to fully hedge against some amount or cost), as well as the hedge ratio (the amount to which the spot moves with the other asset).