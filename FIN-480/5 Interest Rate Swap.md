[[2024-04-26]] #Bonds #Valuation #Derivatives 

### Swaps
An interest rate swap is **a series of forward contracts to exchange floating rate payments for fixed rate payments**.

Suppose a company has borrowed at a floating rate and is now concerned that interest rates will rise, placing the company in financial distress. The company could **hedge the risk of an interest rate rise** by entering into a swap contract to receive floating rate payments and pay fixed payments.
- So accounting for the debt and the swap contract the company has **effectively borrowed at a fixed rate** rather than a floating rate

The **swap rate** is the interest rate used to compute the fixed payments that makes this a fair deal. A fair deal is one in which the **present value of the floating rate payments** is equal to the **present value of the fixed rate payments**.

Yields on government bonds are **a series of 3-month interest rates** that we can lock in today for different time periods. 

Hedging against interest rate movements can be useful, especially for regulated utilities.
- A fall in interest rates flows through to lower prices of regulated utilities
	- If the utility has fixed rate borrowing it can become financially distressed

---
### Computation
1. Compute the **cash flows** under the floating rate part of the swap contract **each quarter**
	- $r_{\text{float},t}=P\cdot \frac{\text{APY}}{4}$
	- $r_{\text{fixed},t}=P\cdot \frac{\text{Swap Rate}}{4}$ (swap rate is computed by goal seek)
	- $\text{CF}_{t}=\text{df}_{t}\cdot P$
1. Compute the **present value of the floating rate cash flows** and the fixed rate cash flows using the **cumulative discount factors** from the futures contracts
	- $\text{df}_{t}=1+r_{t}$
	- $\text{cumulative df}_{t}=\text{df}_{t}\cdot \text{cumulative df}_{t-1}$
	- $\text{PV}_{t}=\frac{\text{CF}_{t}}{\text{cumulative df}_{t}}$
3. Adjust the swap rate with goal seek so the sum of present value of all floating rate payments is equal to that of all fixed rate payments
