[[2024-09-03]] #Valuation #Bonds #DCF

Some definitions beforehand.

```ad-important
**Definition 3.1**: Equivalency

We say that two compounding methods are **equivalent** if the corresponding **growth factors** in 1 year are the same. If one of the growth factors exceeds the other, then the corresponding compounding method is said to be **preferable**.
```

```ad-important
**Definition 3.2**: Effective Rate

For a given compounding method with interest rate $r$, the effective rate $r_e$ is  one that gives the **same growth factor** over a one year period under **annual compounding**.
- For periodic compounding: $(1+\frac{r}{m})^{m}$
- For continous compounding: $e^{r}$
```

---
### Bonds
Bond is a financial security promising the holder a sequence of **guaranteed future payments**.  

#### Zero-coupon Bonds
More specifically, a **Zero-coupon bond** means that the buyer of the bond will receive **face value** $F$ (usually = 100 or 1), known as the face value, on a given day $T$ (usually 1 year), called the **maturity date**.


We define $F\cdot B (t, T)$ to be the value at time $t$ of a zero coupon bond with face value $F$ and maturing at $T$. Note that $B(T,T)=1$ and $B(0,T)$ is the current value. By nature we have $V(0)=P=F\cdot B(0,T)$.

Hence, $$V(t)=F\cdot B (t, T)=F \frac{B(t,T)}{B(0,T)}$$
Note that the principal $P\le F$ for $0 \le t \le 1$ due to discounting.
##### Implied Interest Rate
The Present Value (PV) is $V (0) = \frac{F}{1 + r}$, if $r$ is annual compounding rate. In reality, we use the **market price of bonds** to imply the annual compounding.

The **implied periodic compounding rate** with frequency $m$ is determined by
$$B(t,T)\cdot\left(1+\frac{r_{m}}{m}\right)^{m(T-t)}=1 \implies r_{m}=m (B(t,T)^{\frac{-1}{m(T-t)}}-1)$$
 A
The **implied continuous compounding rate** is
$$B(t,T)\cdot e^{r(T-t)}=1 \implies r=-\frac{\ln B(t,T)}{T-t}$$

#### Coupon Bonds
Besides the face value $F$ due at maturity $T$ , coupon $C$ is paid regularly (annually, semi-annually, quarterly), the last coupon due at $T$.
$$V(0)=PV=\sum\limits_{i=1}^{T} Ce^{-ri}+Fe^{-rT}$$

```ad-note
We calculate the value of a coupon bond at time $t$ as the **sum of the PV** of all future payments to be received in $(t,T]$, **IMMEDIATELY after** the coupon payment $C$ at time $t$.

For instance, in an iterative fashion, for $0 \le t < 1$, $V(t)=V(0)e^{rt}$; and, for $1 \le t <2$, $V(t)=V(1)e^{r(t-1)}$
```

We say a bond sells at **par**, if its price $P$ is equal to its face value $FV$. The coupon rate $i$ is the fraction of the coupon over the face value: 
$$i=\frac{C}{F}$$
A bond with **annual coupons** sells **at par** if and only if the coupon rate $\frac{C}{F}$ equals to $r$. To demonstrate this, let's show a simple example ($T=3$):
$$\begin{align}
PV&=\frac{C}{1+r}+\frac{C}{(1+r)^{2}}+\frac{C+F}{(1+r)^{3}} \\ &=\frac{rF}{1+r}+ \frac{rF}{(1+r)^{2}}+ \frac{rF+F}{(1+r)^{3}} \\ &= \frac{rF}{1+r}+ \frac{rF+F}{(1+r^{2})} \\ &= \dots \\ &= F
\end{align}$$

Since $PV$ is a **monotone function** of $r$, th only $r$ such that $PV=F$ must be given by $\frac{C}{F}$.

---
### Money Market Account (MMA)
The bank acts as an agent to help its MMA customer to buy and sell bonds.

Let $A(0)$ be the **initial investment** in MMA. The bank uses it to buy $\frac{A(0)}{B (0, T )}$ **zero coupon bonds**, i.e., the face value is $1. Then the value of the MMA at time $t$ is
$$A(t)=A(0) \frac{B(t,T)}{B(t,0)}=A(0)e^{rt}, t \le T$$
Where $r$ is the **implied** continuous compounding rate by the bond.

In fact, $A(t) = A(0)e^{rt}$ holds for any $t > 0$, where we assumed that bonds of the same characteristics are traded for the rest of the time.
- At time $T$, the bond expires, so we get **cash** $A (T) = A (0) e^{rT}$ 

In fact, $A(t)=A(0)e^{rt}$ holds for **coupon bonds** as well. 

```ad-note
To demonstrate the above, assume the value of coupon bonds ($n$ years, coupon $C$ annually) at time $0$ is $V (0)$, and we start with $A(0)$ cash.
- At time $t=0$, we buy $\frac{A(0)}{V(0)}$ units of coupon bonds
- At time $t=1$, we **cash the coupon**, and **sell the bond**; then
	- $+C$ from the coupon, and
	- $+V(1)=V(0)e^{rt}-C$ from selling the bond

This yields
$$A(1)=\frac{A(0)}{V(0)}(C+V(0)e^{rt}-C)=A(0)e^{rt}$$
```
