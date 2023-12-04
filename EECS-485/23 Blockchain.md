[[2023-12-02]] #WebApplication 

### Background, Currency
Currency is a medium of exchange.
- **Fiat money**: a currency **WITHOUT** intrinsic value
	- Created by government regulation
	- Valuable because two parties agree it has value
- **Digital currency**: currency available in digital form

---
### Bitcoin
A **decentralized digital** currency. It is a mixture of cryptography, distributed systems, and game theory.
- Invented by Satoshi Nakamoto in 2009
- First cryptocurrency: a fiat currency without government
- Solve the digital currency “double spending” problem

```ad-summary
**Key difference between government backed currency and Bitcoin**

![[Pasted image 20231202205935.png|500]]
```

A Bitcoin is an information object that represents value.
- Represented as **a chain of digital signatures** over the transactions that the Bitcoin participated in

The owner of a Bitcoin is a **Bitcoin address**, which is a public key (AKA Bitcoin Wallet).
- Owner of a Bitcoin creates a transaction by signing a statement that describes transfers ownership from one Bitcoin address to another

For Person A to pay person B, A will need to **broadcast** the transaction to all Bitcoin nodes.
- Nodes are just computers run by Bitcoin people 
- All nodes must agree on a **sequence of transactions** (blockchain)
- The process of agreement is called **Bitcoin Mining**

---
### Blockchain
Traditionally, to avoid double spending, banks use the ACH system for transferring money.
- Centralized balance sheet
- Verify identity of the sender and recipient bank
- Verify account amounts

Blockchain is essentially a **distributed database with a shared protocol** for **multiple writers** who *don’t trust each other or a central authority.*

```ad-summary
Blockchain useful when #Exam 
- Database is shared with multiple writers
- Users don’t trust each other  
- Users don’t trust a central authority
```

In Bitcoin transaction, participants engage in **peer-to-peer protocol** that shares transaction data.
- Collectively build a log of every Bitcoin transaction ever
	- The log is built from blocks
	- The logs form a **distributed ledger**

#### Block
Each block has:
- A sequence id
- A timestamp  
- A nonce  
- Cryptographic **hash of the previous block**
- Various metadata fields  
- Some actual transactions

Blocks are connected in a blockchain.
- Always a **backward hash link** - can traverse it way back to the *genesis block*
- The set of blocks form a tree, with the genesis block at the root; possibly multiple branches
	- The longest path to the genesis block is called the **valid** main chain

![[Pasted image 20231202213140.png|500]]

```ad-example
**The blockchain in action**
- Suppose Alice wants to give Bitcoins to Bob  
- Alice creates and signs a **transaction object** that transfers the Bitcoin from herself to Bob
- She sends it to all peers in a **peer-to-peer network**
- The peers rebroadcast it, so soon, everyone knows about the transaction
- Any participant who also mines on the blockchain can now attempt to **create a block** that contains all the transactions known (including Alice’s transfer)
	- If this works, then the Alice-> Bob transfer is added to the global shared log of transactions
```

---
### Mining
Bitcoin miners are **paid** to operate the blockchain to motivate people overseeing the distributed ledger. 
- They run servers to execute Bitcoin servers
- Block creator is allowed to insert some **reward** Bitcoin transactions - this is how miners earn Bitcoins and how Bitcoins are created
- Anyone can mine

Bitcoin design decisions focus on building and maintaining a blockchain that is **agreed-upon**.
- Only rewards miners to focus on consensus branch (agreed-upon) to focus on achieving consensus

Transactions are broadcast to all nodes. In each round a random node **signs a block of new transactions**, including the hash of the previous block.
- Other nodes accept the block if all transactions are valid
	- If all transactions agree
- Invalid blocks are ignored, next node repeats this block
    
Hence, the longest chain is considered canonical, which leads to a valid canonical chain with “honest majority”.

Why does a new block have to be mined? The block is only valid if the the **hash** of the block is **under a target value**.

#### Proof-of-work
Formulating a block is a proof-of-work puzzle: hard to compute, easy to validate.
- Solution is evidence
- The target value can be adjusted to be harder/easier to mine, tuned to be ~10 minutes
- Helps **cap** the rate at which currency is created, preventing runaway inflation

The branch with more computational power will grow more quickly.
- Bitcoin says that only the longest chain should be treated as valid
	- Eventually, **ONE CHAIN** wins
	- Or a **fork** is maintained and a new crypto is born

```ad-tldr
Why give miners such strong incentives to formulate blocks?
- We want to bring more **hashing power online**, which reduces one's ability to forge an alternate history blockchain
	- In other words, you can start to ignore transactions if you control 51% of the hashing power
```

Bitcoin reward halves every 4 yhaears, which controls the inflation of Bitcoin.

---
### Altcoin
Altcoins are **alternative cryptocurrencies**, launched after Bitcoin.
- E.g. Ethereum, Ripple

A cryptocurrency exchange allows customers to trade different cryptocurrencies.
- E.g. Poloniex, Bitfinex

There are two main motivations behind the creation of Altcoin.
1. Different blockchain features
	- Different blockchain comes with different features
		- Faster block times / larger block sizes
		- Alternative proof of work
	- Alternative transaction types
2. Political and social causes
	- **Forks** of existing cryptocurrencies (branching out)
	- Big disagreements over small block vs. Large block

```ad-summary
**Reasons for cryptocurrencies**
- Distrust in centralized banks or governments
- Predictable policy
	- Code and protocols instead of malleable laws
- Removing financial gatekeepers

**Reasons against cryptocurrencies**
- Trust over anonymous people
- Bugs in code
- Larger power consumption and environmental impact
```
