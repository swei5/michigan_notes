[[2023-11-21]] #WebApplication 

### Motivation
Ads are the business model behind most of the web.
- Before the web
	- Content known in advance
	- Approximate the number of people you reach
	- Advertisers purchased newspaper space/air time well **in advance**
- With the web
	- Content not known in advance
	- Measure audience more accurately
	- Ads are sold *just in time*
		- Use **automated auctions** to select the ad and price
		- **Pay per click**

---
### Auction in Real World
We observe that it is expensive to run an auction and get the attention of multiple potential buyers in a short window. It is commonly used for
- Commodities, livestock
- Art, antiques, rare items

Auctions become much cheaper to run **online**, particularly if there is no human in loop.

---
### Auction, Terminology
There are lots of different kinds of auctions, which favor different outcomes. This is sometimes called **mechanism design**, creating an auction system can be highly mathematical.

#### Players
AKA agents, including a seller and a set of bidders. It is commonly assumed that
- Bidders each know their valuation of the thing
- Bidders do **NOT** know each others' valuations
- Seller does **NOT** know the bidders' valuations of the thing

#### Types
There are two broad categories:
- **Open auction**: everybody sees the bids
	- Ascending-price ("English" auction)
		- Price goes up and up, until one bidder left
		- Best-known type
	- Descending-price ("Dutch" auction)
		- Price goes down from high level, until someone agrees
- **Sealed-bid auction**: bids are secret at once
	- First-price
		- Highest bid wins, and pays bid price
	- Second-price (AKA "Vickrey" auction)
		- Highest bid wins, pays 2nd-place price

---
### Auction, Analysis
In both English (ascending) and Vickrey auctions, winner is the bidder with highest value, pays 2nd-highest value.
- Technically, English auction winner pays 2nd-highest + minimum bid increment

#### Vickrey Auction
In sealed-bid, second-price auction, bidding your **true value** is always best. Given $v_i$ is the value for object $i$, and $b_i$ is the bid for object $i$. The payoff to the bidder is
- $v_{i}-\max (b_{k})\text{  if }b_{i}>\max (b_k)$
- $0 \text{ otherwise}$

```ad-note
- If $b_{i}>v_{i}$, bidder would pay more than they value it (with negative payoff) so they should never do that
- If $b_{i}<v_{i}$, bidder may fail to obtain object (0 payoff)
```

Hence, the best strategy is $b_{i}=v_{i}$.

In sealed-bid second-price auction, your bid **DOES NOT** directly impact what price you pay. It determines *whether you get to pay*.
- This is same with English auction
- Sometimes we said that 2nd-price auction bidders are *truthful*

#### Proxy Bidding
Takes place in an open auction environment, where 1st-place bidder pays 2nd-place price.
- Mix between Vickrey and English
- Used by Ebay

Ebay automatically bids for users if the current bid is less than the user's maximum bid, until the bidding timeframe ends, and the winner walks away by paying the current bid (second place bid) plus one increment.

##### Sniping
Some people on Ebay use software to "snipe" auctions.
- At an appointed time **immediately before time runs out**, the software raises the sniper’s bid substantially
- This effectively turns a Proxy Bidding auction into a Vickrey Auction, as it *seals bids*

#### Sealed-Bid, First Price Auction
Because the bid **determines the price**, bidding the true value may lead to overpayment.
- Bidders tend to **underbid** in a first-price auction

---
### Auctions for Selling Ads
#### Pay-per-click (PPC) Auctions
Auctions determine which ads you see.

#### Overture PPC auctions
Overture was the first company to do text search ads, even before Google.
- Bidders purchase "keywords"
	- Ads displayed **in order, highest first**
- If the user **clicks** the ad, high bidder **pays** bid price
	- Bids are visible to other bidders

This is a **first-price, open auction**.

It's easy to observe that it’s advantageous to **change your bid rapidly**, as bidders aim to pay just enough to be on top, but no more.
- Bad social effects: lots of time/money spent on bidding

#### Google PPC (AdWords)
Has a few differences from the Overture model:
- Sealed-bid, 2nd-price auction
- Allowed Ebay-style "auto bidding" but other bids are **NOT revealed**
- Displayed ads ranked by **combination of bid amount and ad-quality**
	- Click-through rates are highly dependent on the **ad text**

```ad-example
Imagine the following scenario:
![[Pasted image 20231202193007.png|400]]

If R2D2 bids $2.75, they still pay $1.01 upon click (2nd-price bid) but Neytiri now pays $2.76 upon click.
```

```ad-summary
**Google AdWords, Caveats**

Google AdWords is not exactly a Vickrey (sealed 2nd-price) auction in that
- Google sells multiple items
- Lacks some technical properties of Vickrey
```
