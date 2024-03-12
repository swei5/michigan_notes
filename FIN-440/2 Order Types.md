[[2024-03-07]] #Trading

### Trading Orders 
Let's look at a few definitions beforehand.

```ad-summary
**Terminology Summary**
- **Bid price**: The price specified in a buy order 
- **Ask price** (Offer price): The price specified in a sell order 
- **Best bid** (Market bid): The highest bid price in the market 
- **Best offer** (Market offer): The lowest offer price in the market
- **National Best Bid and Offer** (NBBO): The best bid and offer available anywhere in the U.S. at a point in time
- **Bid-ask spread** (Inside spread): The difference between the best ask and the best bid 
- **Trade price**: The price at which the trade is executed
```

Orders are instructions traders give to brokers and/or exchanges. Orders **ALWAYS** specify:
- Security to be traded 
- Quantity
- Direction (buy or sell),
And may specify
- Price conditions, method, timing, expiration, etc.

#### Market Order
A market order **requests a trade immediately** at the **best available price**.
- Demand immediacy
- Demand liquidity in the form of immediacy
- Execution near certain, but **price is uncertain**
- **Transaction cost** per trade is **1/2 the spread** (the price of immediacy)

Market orders have **market impact when dealers move prices** to find liquidity.

```ad-note
A market is **liquid** when traders can trade when they want to without **having much impact on price**.
```

#### Limit Order
A limit order **specifies a price** and the order will be executed **only at this (or at a better) price**.
- A limit order to **buy** at price $k$ will be executed only at $p \le k$
- A limit order to **sell** at price $k$ will be executed only at $p \ge k$

Standing limit orders wait for execution. Orders are placed in a **limit order book**.

```ad-important
The limit order book illustrates the **supply and demand** of an asset at any given time.
```

```ad-summary
**Limit Order, Termiology**
- Aggressively priced or **marketable**: executable 
- **At the market**: best bid or asked 
- Behind or **away from the market**: not executable
- **In the market**: between bid and ask
```

Limit order **DOES NOT** guarantee that the order will executed. There is some price uncertainty. However, they earn the **bid-ask spread**, if execute. Limit orders allow traders to profit from future contingencies.

Standing limit orders **supply liquidity** by allowing others to trade when they want.

Limit orders are options.
- Standing limit buy orders are put options 
- Standing limit sell orders are call options 

Like other options, the value of the implicit limit order option **increases with volatility and maturity** and it depends on the strike price (i.e., the limit price).

#### Stop Order
A stop order or stop-loss order **specifies a price** $k$.
- A stop **buy** order becomes a **market order** when the stock is traded at $p\ge k$
- A stop **sell** order becomes a **market order** when the stock is traded at $p\le k$

This is useful in **closing out positions** when market conditions become unfavorable, or **chasing the performance** given positive market momentums. They act as **insurance** to drastic changes.

#### Stop-Limit Order 
Combines a limit order and a stop order. Two prices are specified - the **stop price** $s$ and the **limit price** $l$.
- A **buy** order becomes a **limit order** at $l$ when the stock is traded at $s \le p \le l$
- A **sell** order becomes a **limit order** at $l$ when the stock is traded at $l \le p \le s$

In essence, an order **becomes a limit order** once a bid or offer is **made at a price equal to or less favorable than the stop price**.

If the stop price and the limit price are **equal**, the order is called a **stop-and-limit order**.
- The order 

Often stop and stop-limit orders are not supported by the exchange, but by the **brokers**.

---
### Order Instructions 
- **Validity instructions**: validity instructions indicate how long the order **remains open** (good)
	- **Good-till-cancel** orders remain open indefinitely 
	- **Good-until orders** specify an expiration dates 
	- **Day orders** expire at day-end.
- **Quantity instructions**: restrict how large orders can be broken into small trades
	- **All-or-none** orders must be completely filled
	- **Minimum partial fill** restrictions
- **Timing instructions** restrict the execution window
	- Market-on-close orders
		- Some mutual funds like to trade at closing prices 
	- Market-on-open orders
- **Execution instructions** tell the broker how to arrange the trade
	- **Market-not-held** is a market order that the broker need not immediately execute or expose; the broker is expected to use discretion to find the best price

---
### Order Book 
A **limit order book** (LOB) at any point in time contains the **details of those limit orders which are currently valid**, but which have not been executed thus far due to the unavailability of a suitable match. The limit order book is the central structure for many trading venues.

![[Pasted image 20240312102913.png|500]]
