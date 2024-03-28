[[2024-03-28]] #Trading 

### Dealers
A dealer (a market maker or a liquidity provider) is a trader who
- Posts a bid and offer (makes a market)
- Sells to incoming buyers; buys from incoming sellers
- Earns the bid ask spread
- Avoids holding large positions

Dealers **AVOID** holding large positions.

Dealers have an obligation to **continuously quote bids and offers**, and the associated sizes (number of shares), when they are registered market markers for a stock.

Traditional dealers in equity markets: NYSE specialists, DMMs (designated market makers) on the floor of NYSE, Nasdaq MMs, and **de facto market makers**.
- Traders who use algorithms and technology to act like market makers
	- **NOT regulated** like traditional market makers
	- Withdraw from the market completely if they think that trading is too risky

```ad-example
**Example: Dealers and Brokers**

- Customer’s order: “Buy 1,000 GM limit $50.”
- Broker: agent for the customer and conveying the order to the market
	- A broker might send this order to an exchange or to a dealer 
- A dealer executes the order against his own account
```

Many firms have both brokers and dealers (internalizing the trade).

---
### Managing Inventory 
Inventories fluctuate with the order flow. Dealers generally manage their inventories by **adjusting their bid and offer quotes**. In some markets, they may also actively initiate trades to adjust their inventories.
- If there are **too MANY** sellers and **NOT ENOUGH** buyers: **lower ask and lower bid**
- If there are **too MANY** buyers and **NOT ENOUGH** sellers: **raise ask and raise bid**

```ad-note
In a volatile market, designated market makers can **increase bid-ask spread** to reduce risk exposure but **CANNOT exit the market**.
```

Dealers can attract order flow by
- Quoting aggressive prices
- Quoting large sizes
- Advertising
- Paying brokers to send it to them
	- The payments ultimately allow brokers to offer lower commissions
	- Payments for order flow are **feasible when spreads are wide** and the order flow is not well informed

---
### Exchanges and Algorithms 

---
### Reg NMS
With **multiple exchanges** and ATSs trading stocks simultaneously, the question arises how it can be assured that the submitted order is executed at the best bid and offer price across all exchanges. This prompted the SEC to establish Regulation National Market System (Reg NMS) in 2005. 

It has four main components
1. Order protection rule
2. Access rule
3. Subpenny pricing rule
4. Market data rules

#### Order Protection Rule 
Within a single book, orders are usually prioritized by price, visibility, time. When there are two orders on **different books** (fragmented order), any and all of these priorities may be violated. A **trade through** is a violation of price priority.

The order protection rule of NMS says
- Markets shall “establish, maintain, and enforce written policies and procedures reasonably designed to prevent” trade throughs

```ad-warning
The order protection rule of NMS **does NOT** say:
- Trade throughs are prohibited
- Trade throughs should never happen
```

To qualify for protection by the order protection rule, an order must be
- Visible (displayed)
- At the top of the market’s book
	- The orders limit price must be equal to the **best visible bid or best visible offer**
- Of an even lot (quantity must be some **multiples of hundreds**)

The Reg NMS mandates that before a market center executes an order, it must **check the protected bids and offers**. If it appears that the execution would cause a trade through, the market center must either
- Return the order unexecuted
- Route the order or part of the order to a market where it could be executed without causing a trade through

```ad-example
**Example: Order Protection Rule**

Consider the following bid books on exchanges A and B.

![[Pasted image 20240328110701.png|300]]

Assume an incoming limit order: sell 1,000 @47. If we execute the entire order against own book, we would have 300 @50, 400 @48, 300 @47.
- However, this would trade through B's protected bid of 49 for 100 shares (100 @50 is not protected as it's not visible)

To avoid this from happening, **A sends 100 shares (number of protected shares) of sell order to B** and execute the rest against our own book.
- Then we would have 100 @50 on B, 300 @50 on A, 400 @48 on A, 200 @47 on A 
	- This is okay, even though B’s protected 100 shares at 49 are traded through
```

#### NBBO
Stock brokers are required by Reg NMS to execute the retail customer trades at the **NBBO** (National Best Bid and Offer) or better. The NBBO is **updated throughout the day** to show the highest and lowest offers for a security among all exchanges.

NBBO is the bid and ask the **average person** will see. Day traders using **Level 2 market maker** screens see **all the bids and offers for a particular stock**.

To get NBB, we take the max of all the best bids across exchanges; to get NBO, we take the min of all the best offers across exchanges. We define that $$\text{NBBO spread}=\text{NBO}-\text{NBB}$$ When $\text{spread}=0$, the market is **locked**: a trade could occur. When $\text{spread} <0$, the market is **crossed**: there should be an arbitrage.

Locked and crossed markets only arise across different exchanges. Within any single exchange, locks and crosses **DO NOT occur**.