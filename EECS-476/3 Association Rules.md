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

```ad-example
**Example**: Applications

Plagiarism in documents
- Baskets:
- Items:

Detect combinations of drugs that result in particular side-effects
- Baskets:
- Items
```

More generally, the above extends to a general **many-to-many** mapping (association) between two kinds of things. We ask about **connections** among **items**, not baskets.

---
### Association Rules
Association rules are if-then rules about the contents of baskets. 
- $\{i_{1}, i_{2},\cdots, i_{k}\} \to j$ means: if a basket contains all of $\{i_{1}, i_{2},\cdots, i_{k}\}$, then it is *likely* to contain $j$

**Confidence** of this association rule is the probability of $j$ given $\{i_{1}, i_{2},\cdots, i_{k}\}$.
- That is, the fraction of the baskets with $\{i_{1}, i_{2},\cdots, i_{k}\}$ that also contains $j$

Generally we want both **high confidence** and **high support** for the set of items involved.

```ad-note
**Mining Association Rules: Overview**
1. Find all frequent itemsets $I$
2. **Rule Generation**
	- For every subset $A$ of $I$, generate a rule $A \to I\setminus A$
		- Since $I$ is frequent, $A$ is also frequent
		- **Variant 1**: Single pass to compute the rule confidence
		- **Variant 2**: If $A,B,C \to D$ is below confidence, so is $A,B \to C,D$
			- Can generate *bigger* rules from smaller ones
	- Output rules above the confidence threshold
```

To reduce the number of rules we can **post-process** them and only output:
- **Maximal frequent itemsets**: **NO** immediate superset is frequent
	- Gives more pruning
- **Closed frequent itemsets**: **NO** immediate superset has the **same count** ($> 0$)

```ad-example
**Maximal/Closed**

```

---
### Computation Model
Typically, data is kept in flat files.
- Stored on disk
- Stored basket-by-basket

As we go through the basket, we expand baskets into pairs, triples, etc.
- Use $k$ nested loops to generate all sets of size $k$

```ad-note
We want to find frequent itemsets. To find them, we have to count them. To count them, we have to generate them.
```

The true cost of mining disk-resident data is usually the number of disk **I/O’s**.
- In practice, algorithms for finding frequent itemsets read the data in **passes** – all baskets read in turn

Thus, we measure the cost by the **number of passes** an algorithm takes.

For many frequent-itemset algorithms, **main memory** is the critical resource. As we read baskets, we need to count something, e.g., occurrences of pairs of items.
- Simple for reading singletons, as we can usually compute counts in memory
- However, difficult for **pairs** and let alone triplets as number of items increases

```ad-example
**Example**: Memory Limitations

Assume an algorithm that counts all pairs of $n$ items. Given $2$GB ($2^{31}$ bytes) of memory, the upper bound of $n$ so that all pairs fit in the memory is $$\binom{n}{2} \cdot 4 \le 2^{31} \to n \le 2^{15}= 32768$$

We multiply the number of pairs by $4$ because the counter variable takes $4$ bytes (assume data type is integer).
```

The hardest problem often turns out to be finding the **frequent pairs**.
- Why? Often frequent pairs are common, frequent triples are rare

We’ll concentrate on **pairs**, then extend to larger sets.

#### Naive Algorithms
A naïve algorithm is to read file once, counting in **main memory** the occurrences of each pair.
- From each basket of $n$ items, generate its $n(n-1)/2$ pairs by two nested loops
Fails if $n(\text{items})^{2}$  exceeds main memory.

There are two approaches
1. Count all pairs, using a **triangular matrix**
	- Requires only 4 bytes/pairs
1. Keep a table of triples $[i,j,c]$ that represents the count of the pair $\{i, j\}$ is $c$
	- Requires 12 bytes, but only for those pairs with count $>0$

```ad-note
**Triangular-Matrix Approach**

1. Number items $1,2,\cdots,n$
	- Requires table of size $O(n)$ to convert item names to consecutive integers
2. Count $\{i,j\}$ only if $i<j$

The total number of pairs is $n(n-1)/2$ and total bytes is around $2n^{2}$.

In reality, the triangular matrix is **mapped** to a flattened vector.
```

```ad-note
**Tabular Approach**

Total bytes used is about 12$p$, where $p$ is the number of pairs that actually occur.
- Beats triangular matrix if at most $1/3$ of possible pairs actually occur

It may require extra space for retrieval structure, e.g. a hash table.
```

#### A-Priori Algorithm
A two-pass approach (for frequent pairs) called **a-priori** limits the need for main memory. 

The key idea is **monotonicity**: if a set of items appears **at least** $s$ times, so does every subset of the set. 
- **Contrapositive** for pairs: if item $i$ does not appear in $s$ baskets, then no pair including $i$ can appear in $s$ baskets
- **Anti-monotonic property**: if an itemset violates a constraint, so does any of its **superset**

**Pass 1**: Read baskets and count in main memory the occurrences of each item.
- Requires only memory proportional to the number of items

Items that appear at least $s$ times are the **frequent items**.

**Pass 2**: Read baskets again and count in main memory only those **pairs both of which** were found in Pass 1 to be **frequent**.
- Requires memory proportional to **square of frequent items** only (for counts), plus a list of the frequent items (for storage)
![[Pasted image 20250124155508.png|400]]
For each size of itemsets $k$, we construct **two sets** of $k$ -sets (sets of size $k$).
- $C_{k}$ is **candidate** $k$ -sets - those that might be frequent sets (support $\ge s$) based on information from the pass for itemsets of size $k-1$
- $L_{k}$ is the set of truly frequent $k$ -sets

![[Pasted image 20250124155801.png|450]]

```ad-note
To find itemsets of size $k$, we need $k$ passes. 
- Needs room in main memory to count each candidate $k$-tuple
- For typical market-basket data and reasonable support (e.g., 1%), $k=2$ requires the most memory

Many possible extensions:
- Association rules **with intervals**
	- E.g. Men over 65 have 2 cars
- Association rules when items are in a **taxonomy**
	- E.g. Bread, Butter $\to$ Fruitjam
- Lower the support $s$ as itemset gets bigger  
```

#### PCY (Park-Chen-Yu) Algorithm  
**Pass 1**: In addition to item counts, maintain a hash table with as many buckets as fit in memory.
- Keep a count for each bucket into **which pairs of items** are hashed
- For each bucket just keep the **count**, not the actual pairs that hash to the bucket

```python
for each basket:  
	for each item in the basket: # Same as A-priori
		add 1 to item’s count;  
	for each pair of items:  
		hash the pair to a bucket;  
		add 1 to the count for that bucket;
```

Pairs of items need to be **generated** from the input file; they are not present in the file. We are not just interested in the presence of a pair, but we need to see whether it is present at least $s$ (support) times.

```ad-note
If a bucket contains a **frequent pair**, then the bucket is surely **frequent**.
- However, even without any frequent pair, a bucket can still be frequent; So, we **CANNOT** use the hash to **eliminate** any member (pair) of a frequent bucket

Nevertheless, the converse is true - for a bucket with **total count** less than $s$, **NONE** of its pairs can be frequent.
- Pairs that hash to this bucket can be eliminated as candidates (even if it contains 2 frequent items)
```

**Between Passes** (1.5)
Replace the buckets by a **bit-vector**:
- **1** means the bucket count exceeded the support $s$ (frequent bucket)
- **0** means it did not

$4$ -byte integer counts are replaced by bits, so the bit-vector requires $1/32$ of memory. 

Also, we decide which **items** are frequent and list them for the second pass

**Pass 2**: Count all pairs $\{i,j\}$ that meet the conditions for being a **candidate pair**:
1. Both $i$ and $j$ are frequent items
2. The pair $\{i,j\}$ hashes to a bucket whose bit in the **bit-vector** is **1**

Both conditions are necessary for the pair to have a chance of being frequent.

![[Pasted image 20250124161618.png|450]]

