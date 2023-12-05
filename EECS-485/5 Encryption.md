[[2023-05-11]] #WebSecurity #Webpage 

### Network Security
When two parties communicate over a network, we would like to avoid any illegal or intentional from one's adversary
- Reading (eavesdropping) of data transmitted
- Modification or deletion of data
- Injection of new data

```ad-important
Some desirable properties to have include
- **Confidentiality**
	- Adversary should **NOT UNDERSTAND** message
- **Sender Authenticity**
	- Message is really **from** the purported sender
- **Message Integrity**
	- Message **NOT MODIFIED** between send and receive
```

^ff793a

This can be done by **encryption**. Plain texts are encrypted using an **encryption key** into cipher text; then, using a **decryption key**, we can translate cipher text back to plain text.

---
### Encryption
#### Symmetric Encryption
Encryption and decryption key are the same: $K_{\text{enc}}=K_{\text{dec}}$.

**AES** (Advanced Encryption Standard) is the most common symmetric cipher.
- Block cipher (128 bits)
- Security - no effective attacks found yet since nobody knows how to break it

```ad-summary
Strengths and weaknesses of symmetric encryption
- **Strengths**
	- Fast
- **Weaknesses**
	- Sharing the key is difficult
	- If you build a key together (Diffie-Hellman), how do you know you are doing it with the right other person?
```

#### Asymmetric Encryption
Two keys that are **inverses** of each other - one being **public**, the other being **private**. It allows us to do two things.

##### Encryption
Sending a message only one person can read.
- Encrypt: plain text $\to$ cipher data with **public key**
- Decrypt: cipher data $\to$ plain text with **private key**
![[types-of-encryption-asymmetric-encryption.png|500]]

##### Signing
Proving that it was **ACTUALLY** some user that sent a message. If `user_a` were to send a message to some other user:
1. Encrypt (sign) with `user_a` 's **private key**
2. Decrypt (verify) with `user_a` 's **public key**
![[Private_key_signing.svg.png|400]]

To design such asymmetric cryptography, we need
- Mathematical way to make two related keys
- **Hard** as possible to figure out private key
	- Assuming everyone knows public key
	- Idea: $b=f(a)$, such that it is hard to find $a$ given $b$

One example is the **trapdoor** function:
$$n=pq$$
where $p,q$ are primes. Given $p$ and $q$, it's easy to compute $n$, but not the reverse.

In this way, only product of primes is ever exposed, as it is computationally extremely challenging to recover original primes. 
- Both public, private keys **require original primes** to compute

---

### Public Key Infrastructure
Asymmetric cryptography depends on **sharing public keys**.

```ad-question
When sharing public keys, for instance, how do you know that Amazon's public key actually comes from Amazon?
```

One way to go about this problem is by using **certificate authorities**. ^e0821c

A **certificate authority** or **certification authority** (**CA**) is an entity that stores, signs, and issues [digital certificates]( https://en.wikipedia.org/wiki/Public_key_certificate "Public key certificate"). A digital certificate certifies the ownership of a public key by the named subject of the certificate.
- Companies manage these
- E.g. signs certificates used in HTTPS

![[ca-diagram-b.png|500]]

---

### Hash Functions
It's always a **BAD** idea for server **to store** password in database.
- If someone has a copy of the database, we're screwed

A better idea will be for the server to **hash password** using a **one-way hash function**.
- If someone gets the database, they don't get the passwords

```ad-example
We can use `SHA-512` to hash a simple password into something that's impossible to decode:

```python
import hashlib  
m = hashlib.sha512('bob1pass') 
password_hash = m.hexdigest() 
print(password_hash)
# prints out af1bd47889bff89ccc889bc2aa61437c2ac90ee411618645bd4a dbca1e02f8a277729093ea8ac094d3265352b75b12af1b4a50ed d8fc5783cc0fac0411cde8c2
```

#### Rainbow Table
A precomputed table for **caching** the outputs of a cryptographic hash function, usually for cracking password hashes.
- This can be used use this against many different databases

![[Simple_rainbow_search.svg.png|500]]

In order to protect against rainbow table, we have to make sure that different people in your database with the **same password** have **different hashes**.
- An easy solution will be to add a random number to each password
	- E.g. `hash(password00)` $\to$ `0x39f8e`, but `hash(password3c)` $\to$ `0xff23`
	- This random number is called a **salt**
		- Don't have to be secret
		- Different for every user
