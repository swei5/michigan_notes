[[2023-05-11]] #Encryption #Webpage 

### Network Security
When two parties communicate over a network, we would like to avoid any illegal or intentional from one's adversary
- Reading (eavesdropping) of data transmitted
- Modification or deletion of data
- Injection of new data

Some desirable properties to have include
- **Confidentiality**
	- Adversary should **NOT UNDERSTAND** message
- **Sender Authenticity**
	- Message is really **from** the purported sender
- **Message Integrity**
	- Message **NOT MODIFIED** between send and receive

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
![[types-of-encryption-asymmetric-encryption.png|500]]

##### Signing
