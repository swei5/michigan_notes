[[2023-05-06]] #HTML #CSS #Webpage

### Overview of Class
How does the Web work?
- Front end
	- Web pages
	- Web browsers
- Back end - infrastructure
	- Servers
	- Routers
- Algorithms
	- Search
	- Recommendations

---

### HTML
Web pages are written in HTML, browser translates it to what you see.

```html
<!DOCTYPE html>  # Indicating this is HTML
<head>
	<title>Web page</title>  # Each tag needs to be opened/closed
</head>
<body>
	<h1>Hello world!</h1>
</body>
```

Other common HTML tags we've seen include
- `<a href="google.com">some text</a>`
	- Here `href` is an **attribute** of tag `<a>`
- `<h1>`, ..., `<h6>`
- `<p>`
	- Paragraph, starts a new line
- `<ul>`

**Escape characters** begin with `&` and end with a `;` .
- `&gt;` - greater than
- `&lt;` - less than
- `&aps;` - ampersand `&`

#### Dom Structure
HTML has a tree structure (Document Object Model).
```html
<html>
	<head></head>
	<body>
		<p>Hello world!</p>
		<p><a href="umich.edu">UMich</a></p>
	</body>
</html>
```

```ad-example
The above snippet of code can be translated into a tree.
![[Pasted image 20230507183742.png|450]]
```

---

### CSS
Enables us to add styles to web pages, usually a **separate file** that is linked in the header of HTML.
- Color
- Layout

Let's show a simple example.
```html
<h1>Title</h1>
<p>Text</p>
```

The HTML code is supported by the following CSS code.
```css
h1 {
  border: 1px solid blue; 
  /* key: val */
}

p {
  color: red;
}
```

Other HTML features commonly associated with CSS include
-  `<div>`
	- Like a **block** or box, always starts on **new line**
- `<span>`
	- Smaller part of **text**, **DOESN'T** start new line

#### `class` and `id`
These are attributes that help **link up** HTML and CSS.
- `class` is used for one or more things, whereas `id` is unique on the entire webpage

Let's again demonstrate using an example.
```html
<div class="outline">div 1</div>
<div class="outline" id="second">div 2</div>
```

Accompanied by CSS code,
```css
div {
  color: blue;
}
/* . corresponds to class, # corresponds to id */
.outline {
  border: 1px solid black;
}
#second {
  color: red;
}
```

will produce the following.
![[Pasted image 20230507184720.png|200]]

---

### URL
Stands for **Uniform Resource Locator**, basically what we type in the address bar in the browser. It tells a web browser to make a **request**.

We will now introduce the concepts of **protocol**, **server**, **path**, and **port**.

For example, in `https://en.wikipedia.org/wiki/URL`
- `https://` is known as the **protocol**
	- What *language* is used for Request-response cycle
	- `https` is the secured version of HTTP
- `en.wikipedia.org` is known as the **server**
	- Which computer we are communicating with
	- `localhost` or `127.0.0.1` means the **same computer**
- `wiki/URL` is the **path**
	- Like a file path
	- If nothing is specified, the default is `index.html`
		- `http://localhost:8000/` is same as `http://localhost:8000/index.html`

In `http://localhost:8000/index.html`, `8000` is known as the **port**.
- When there is **more than one program** on the server computer might communicate with the network - port specifies **which one**

Port can be implied.
- `HTTP`: 80
- `HTTPS`: 443

#### Query, Fragment
Queries are the **data sent** to the server inside the URL.
- It is represented by key-value form

```ad-example
In https://en.wikipedia.org/w/index.php?title=URL&action=edit, there are two queries
- `title=URL`, and
- `action=edit`

Intuitively, `?` specifies the beginning of the query, and `&` is used to separate two queries.
```

Fragments are only used by **web browser**, not server. It is used for **creating links** to **headings** on page.
- https://en.wikipedia.org/wiki/URL#Syntax takes us to the section under `#Syntax` on the page

Escape sequences in URL begin with `%`. `%20` means space.

---

### HTTP
On the web, clients send **HTTP requests** to the server and get **HTTP responses**.

![[HTTP-request-response-model.png|400]]

Clients **ALWAYS initiate** in such process. Before diving in depth, we need to understand a few concepts and words.

#### Request
Let's begin with an example.
```
> GET / HTTP/1.1
> Host: cse.eecs.umich.edu
> User-Agent: curl/7.54.0
> Accept: */*
```

Here, `GET` is the verb, `/` is the path, and `HTTP/1.1` is the version. `Host`, `User-Agent`, and `Accept` are **headers**.
- `HOST`: distinguishes between DNS names sharing a single IP address
- `User-Agent`: browser making the request
- `Accept`: which content (file) types the client will accept

There are a number of common verbs.
- `GET`: send a file (to the browser) ^12804f
- `HEAD`: get only the **headers**
	- Way to see if a file **has changed** before asking for it to be sent another time
- `POST`: send data (to the server)
	- E.g. log-in form

#### User Agents
When a **browser** visits a page, it identifies Itself with a `User-agent` string.

An example from Google Chrome could look something like:
```
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36
(KHTML, like Gecko) Chrome/48.0.2564.103 Safari/537.36
```

Here `AppleWebKit` is Safari, `Mozilla` is Firefox, `KHTML` is Linux - the user agent just simply puts everything in there in case it affects what the browser get sent.

#### Response, Status Code
Let's again show a quick example.
```
< HTTP/1.1 200 OK
< Date: Tue, 12 Sep 2017 20:04:20 GMT
< Server: Apache/2.2.15 (Red Hat)
< Accept-Ranges: bytes
< Connection: close
< Transfer-Encoding: chunked
< Content-Type: text/html; charset=UTF-8
```

Look very similar to a request in terms of formatting, except that response starts with a **status code**.
- `1XX`: informational
- `2XX`: successful
	- `200 OK`
- `3XX`: redirection error
- `4XX`: server error
	- `404 Not Found`
- `500`: server error
	- Python log

Content-type describes what **kind** of file it is.
- Text/ `HTML`
- Image/ `png`

Character encoding is typically `UTF-8`, which is the most common and supports non-English language (more comprehensive than ASCII).

Most response headers are optional and describe how the data will be **transferred**.
