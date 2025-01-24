[[2025-01-24]] #DataMining 

Our goal in this section is to discover and identify hidden patterns in data.
- E.g. Identify items that are bought together by sufficiently many customers
	- **Approach**: Process the sales data collected with barcode scanners to find dependencies among items

The Market-Basket Model contains
- A large set of **items**, e.g., things sold in a supermarket.
- A large set of **baskets**, each of which is a **small set** of the items, e.g., the things one customer buys on one day.

The **support** for itemset $I$ is the **number of baskets** containing all items in $I$.
- Sometimes given as a percentage of the baskets

Given a **support threshold** $s$, a set of items appearing in at least $s$ baskets is called a **frequent itemset**.

