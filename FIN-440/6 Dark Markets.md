[[2024-04-11]] #Trading 

### Dark Pools
A market center (e.g., NYSE, NASDAQ) is a venue in which you can execute orders to buy or sell shares. 
- Generally displays its bids and asks, considered **lit**

**Dark pools** (dark markets) are a subset of all market centers. Dark pools are essentially **private exchanges** for trading securities and are regulated under Reg. ATS.
- **DOES NOT** display bids and asks

Post-trades, exchanges are dark pools are the same as it is **required that transactions and volumes** need to be reported to FINRA's TRF

Dark pools were largely created to allow **institutional investors** to execute large volume trades **WITHOUT creating an unfavorable impact on market prices**.
- Creating trading opportunities that are unavailable to the general investing public
- Ability to conceal their subscribers’ trading activities

Some lit markets allow dark trades.
- Hidden orders 
- Internalized orders 

In dark markets, **all trades** are dark.
- [[3 Order-driven Markets#^8d69de|Crossing sessions]]
- Continuous dark pools (more popular)

#### Structure
Dark pools support many different order types and attributes.
- Some use the **mid-point pricing**: The order is pegged to the prevailing mid-price of the NBBO quote
	- Price is usually a function of NBBO
- **CAN exclude** investors 
- Flexible in [[3 Order-driven Markets#^805272|precedence rules]]
	- Price is usually still most prioritized
	- Tiering: favoring some clients than others or can be done on the basis of past dark pool performance

All attributes are **NOT uniform** across all pools.

```ad-summary
**Dark Pools, Pros and Cons**

**Pros**
- By crossing at the midpoint, dark markets save investors money
- Dark pools prevent other traders from learning about investors’ trading plans

**Cons**
- Dark markets “free ride” on bids and offers in the lit markets 
- As traders move to dark markets, the lit markets will have less volume/information: **liquidity** will be hurt and **transaction costs** increase
```

#### Price Manipulation
Trading to deliberately move the price, to establish an artificial price, a price that does not reflect true supply and demand.

There are various ways to accomplish that
- Quote manipulation 
	- The buyer in a dark pool knows that any execution will be priced at the NBBO midpoint 
	- The buyer can **lower the midpoint** by submitting an **aggressive sell limit order to a lit market** (multiple of 50)
	- After the dark pool execution, the sell limit order is cancelled
	- **ILLEGAL**
- Front running
	- **LEGAL** as long as you can learn the information in a legal way

Players can send a small **Immediate or Cancel Order** (IOC) order, say 100 shares, to the pool. If the order is executed, and they can learn about the **existence of a (possible) big order in the pool**.

Dark pools use various methods for avoiding gaming and information leakage.
- **Large blocks pools**: 
	- By **enforcing large minimum quantity** for each order the pool prevents the gamer ability to detect large orders without taking significant risk
- **Limiting participants**