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
- All nodes must agree on a **sequence of transactions**
- The process of agreement is called **Bitcoin Mining**

---
### Blockchain
Traditionally, to avoid double spending, banks use the ACH system for transferring money.
- Centralized balance sheet
- Verify identity of the sender and recipient bank
- Verify account amounts

Blockchain is essentially a **distributed database with a shared protocol** for multiple writers who don’t trust each other or a central authority.

In Bitcoin transaction, participants engage in **peer-to-peer protocol** that shares transaction data.
- Collectively build a log of every Bitcoin transaction ever
	- The log is built from blocks

#### Block
Each block has:
- A sequence id
- A timestamp  
- A nonce  
- Cryptographic **hash of the previous block**
- Various metadata fields  
- Some actual transactions

Blocks are connected in a blockchain.
- Always a backward hash link - can traverse it way back to the *genesis block*
- The set of blocks form a tree, with the genesis block at the root; possibly multiple branches

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
Bitcoin miners are **paid** to operate the blockchain. 
- They run servers
- Block creator is allowed to insert some **reward** Bitcoin transactions - this is how miners earn Bitcoins and how Bitcoins are created

