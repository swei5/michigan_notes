[[2024-03-12]] #Trading 

**Orders are the fundamental building blocks** of trading strategies. Markets may be classified based on the execution system that implements orders.

An **execution system** is a set of procedures for matching buyers and sellers. In **pure order-driven markets**, execution system is only dependent on orders. Examples include 
- Tokyo Stock Exchange 
- Toronto Stock Exchange 
- Euronext Paris  
- Most futures markets
- NYSE (partially, with the exception of approved dealers)

Almost all new exchanges and almost all electronic exchanges are order-driven and trading rules affect order submission strategies.

### Order-driven Markets 
In an order-driven market, buyers and sellers of securities are able to **place orders for securities they wish to purchase or sell WITHOUT the intermediation** of a dealer.
- Participants include **public traders** and **dealers**
	- In a pure order-driven market however, a dealer is on par with any other public trader

```ad-note
Most order-driven markets are **auction markets**. In these markets the **trading rules** define the process by which buyers seek the lowest available prices and sellers seek the highest available prices. This is called the **price discovery process**.
```

Primarily, there are two types of markets.
- **Continuous markets**
	- Traders may trade at **ANYTIME** while the market is open
	- Traders may continuously attempt to arrange their trades
	- NYSE from 9:30-4
- **Call markets** ^8d69de
	- Traders may trade in call markets only when the **market is called**
		- You may have all securities called at the same time or only some; the market may be called several times per day
	- The opening price and closing price at NYSE is an example of this market
	- Two types of order:
		- Market on Open (MOO)
		- Limit on Open (LOO)

```ad-summary
**Continuous Markets and Call Markets, Pros and Cons**
- **Pros for continuous markets**
	- Traders can arrange their trades whenever they want 
	- Information may be incorporated very fast into prices 
- **Cons for continuous markets**
	- Can be volatile
- **Pros for call markets**
	- Focus the attention of traders on the same security at the same time 
	- Less volatility and higher liquidity
- **Cons for call markets**
	- Information may need a lot of time to be incorporated into prices
```

In order-driven markets, trading rules specify how trades are arranged.
- The **Order Precedence Rules** state as to how buy and sell orders should be arranged and **matched**
- The **Trade Pricing Rules** are used to determine the **price** at which a trade is to be executed

These rules vary across markets.

#### Order Precedence Rules 

^805272

**Market orders** have the **highest** priority. Then, the execution price for such orders is determined as follows:
- A market buy/sell order will be executed at the best available price from the **standpoint of the trader** (LOB)

In order to ensure that market orders get executed at the best available prices, the **unexecuted but valid limit orders** at any point in time must be **sorted** according to Priority Rules.
1. **Price priority rule**
	- A limit buy order with a higher limit price ranks higher
	- A limit sell order with a lower limit price ranks higher
2. **Time priority rule**
	- All orders at a price are ranked by their **arrival times**

![[Pasted image 20240312110250.png|500]]

Other priority rules include
- **Display priority rule**
	- **Displayed quantities** at a given price generally have precedence over the undisplayed quantities (if system allowed)
- **Public order priority rule**
	- **Public orders** have precedence over member orders at a given price
- **Size priority rule**
	- Some markets give precedence to small orders, other markets favor large orders 

```ad-summary
The complete precedence hierarchy is given by price priority, display precedence at a given price, and finally time precedence among all orders with the same display status at a given price. 

These rules give traders incentives to improve price, display their orders, and arrive early if they want to trade quickly. These incentives **increase market liquidity**.

Match quantity is $\min(n(A), n(B))$.
```

#### Trade Pricing Rules
Continuous auction markets maintain an order book. The buy and sell orders are separately sorted by their precedence. When a new order arrives, the system tries to **match the best order** with orders on the other side.
- If a trade is possible, e.g., the limit buy order is for a price at or above the best offer, the order is called a **marketable order**
- If a trade is not possible, the order will be **sorted into the book** according to its precedence

Once the orders are **matched**, the trading system then uses its trade-pricing rules to determine trade prices. The three rules that various order-driven markets use to price their trades are:
- 1. **Discriminatory pricing rule**
	- The **limit price** of the standing order dictates the price for the trade
	- If the incoming order fills against **multiple standing orders** with different prices, trades will take place at **multiple prices**
	- Allows a large arriving trader to **discriminate among standing limit orders** by **filling the most aggressively priced orders first** at their limit prices and then filling less aggressively priced orders at their less favorable
	- Most continuous order-driven markets
1. **Uniform pricing rule**
	- All trades execute at the **same price** (clearing price)
	- The **last match** that leads to a feasible trade determines the **clearing price**
	- Most [[#^8d69de|call markets]] and used for opening/closing markets
	- The **market clearing price** is the price where **supply equals demand**
	- The market clearing price **maximize the volume of trading**

```ad-note
If trading systems did not use this pricing rule, large traders would **break their orders into pieces** to price discriminate on their own.
```
