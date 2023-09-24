[[2023-05-10]] #SQL #Webpage 

### HTTP
#### HTTP 1.0
- New connection for **EVERY** file

For the following HTML file, two connections need to be made.
```html
<!-- index.html -->
<html>
<body>
	<img src="block_m.png"/> 
</body>
</html>
```

1. Client (browser) requests `index.html` to the server, the server sends the HTML file back (then the browser renders it); **cuts the connection, restart a new one**
2. Client requests `block_m.png` to the server, the server sends the image file back

#### HTTP 1.1
- Can **reuse** one connection for multiple requests and responses. 
- However, **ONE** request/response **PER FILE**
	- Hence, we still need two connections for the example above

#### HTTP 2.0
- The server can tell that certain files always get **sent together**, and can respond with **multiple files** for **one request**
	- The example above would only require **ONE** connection
		- Client requests for `index.html`, server sends back both `index.html` and `block_m.png`

---

### Sessions
In simple terms, session enables a website to know whether if multiple **requests** came from the **same user**.
- E.g. log-in on one web page - the site remembers you are logged in on **other pages**
- E.g. shopping cart

```ad-question
How can we do that? HTTP is **stateless**. That being said, no memory is being stored between requests or connections.
```

The solution is **cookie**. 
- Every HTTP request **includes some data** that the server gives the client

To illustrate this process, we can take a look at both clients' and server's perspectives.

```ad-example
Client perspective:
![[Pasted image 20230511002827.png|600]]

Server perspective:
![[Pasted image 20230511002858.png|600]]
```

**Server** is in charge of creating session by setting a cookie.
- When client sends some kind of request to the server the first time, the server responds with some **unique identifier** (cookie) along with its response
- Then, both the server and browser stores the cookie in its database 
	- For server database, it stores `{cookie:user}`, e.g. `{abc123, John}`
	- For web browser, it stores `{site:cookie}`, e.g. `{amazon.com, abc123}`
- Later on, the client sends requests **along with** the cookie

---

### Securing Cookie
Recall that cookies are delivered from the server to the client.
- HTTP is an **insecure** way of transit as all the source data are not protected (both the cookie and user's data/pin can be seen)
- HTTPS adds a layer of **encryption** to user's information, thereby making the cookie information more secure

We should always be mindful of client **modifying cookies** - this could be dangerous as it induces information leak. There are usually two ways to prevent this
- **Signature**: add a number to the end that **ONLY server** knows how to generate/check
- **Encryption**: scramble cookie so client can't read or change it

Finally, let's use the `flask` framework to denote how a cookie is created in the background.

```python
import flask  
app = flask.Flask(__name__)

app.secret_key = b'uAy\x9d\x08[\x12\x8d\x9d\x1f\xbar\x86A\x9fpQy4\x05)v04' # random signature for cookie

@app.route('/')
def index():
	if "user" in flask.session:  
		user = flask.session["user"]  
		app.logger.debug("Get user=%s", user)  
		return "<html><body>Hello {}</body></html>".format(user)
	else:
        flask.session["user"] = "awdeorio" # sets cookie
		app.logger.debug("Set user=%s", user)  
		return "<html><body>Logging in ...</body></html>"
```

---

### Database
We don't want to store information in **variables**, as when programs restart, all variables are destroyed. Now, SQL (structured query language) is our friend in storing and accessing data from a **database**. 

Specific syntaxes for `CREATE`, `INSERT`, `SELECT`, and `JOIN` will not be covered here as they are fairly basic. We do, however, need to use Python to access `sqlite` databases.