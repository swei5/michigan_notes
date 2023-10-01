[[2023-09-30]] #Webpage #JavaScript #React

### Server-Side versus Client-Side
- Server-side dynamic pages
	- Server response is the output of
		- A function
		- **Good** for **database** access
			- Server function runs SQL query
		- **Bad** for **user interaction**
			- Refresh required
- Client-side dynamic pages
	- JavaScript running in the client’s web browser modifies the DOM
	- **Bad** for database access
		- JS runs on client, not server
	- **Good** for user interaction
		- Modify the DOM **WITHOUT** **refresh**

```ad-question
Question: can we have both?
```

Our goal here is to link client-side dynamic pages to server-side dynamic pages via a [[7 REST APIs|REST API]]. In details,
- JavaScript running in the client’s web browser makes a **REST API request** and then modifies the DOM using data from the response
- Server response is the output of a function which **runs an SQL query** and returns the **[[7 REST APIs#^b4608e|result in JSON format]]**

A client-side application usually uses **both** client-side dynamic pages and server-side dynamic pages. This is sometimes called "Full stack" development.

---
### Client-Side Applications
Below is an outline of how the application functions.
1. Client requests root (index) page
	• Server responds with static HTML
2. Client loads HTML into DOM
2. Static HTML includes a `<script>` tag with file path to JavaScript
3. Client requests JavaScript source code
	- Server responds with static JavaScript file
1. Client executes JavaScript
2. JavaScript makes request to REST API
	- Server responds with `JSON`
1. JavaScript parses `JSON` into an object
2. JavaScript uses data in object to modify the DOM
3. Page updates

```ad-tldr
Our goal is to modify the **DOM** using data from the REST API.
```

Let's demonstrate this using an example.

Say we want to build a webpage that looks like this.

![[Pasted image 20230930154907.png|400]]

An old-fashion way to go about it is through plain HTML text:

```html
<html>
	<head></head>
	<body>
		<div>
			<p>awdeorio has 0 snippets</p>
			<p>jflinn has 0 snippets</p>
		</div>
	</body>
</html>
```

If we want to, we'd also be able to include JS code into our play, then we could have something that looks like this:

```html
<!-- index.html, same thing -->
<html>
	<head></head>
	<body>
		<div id="JSEntry"></div>
		<script src="/static/users.js"></script> <!-- entry point -->
	</body>
</html>
```

From which we separate the JS source code. Here, the JS code is in charge of adding **nodes** to the DOM which changes the content of the webpage, as rendered by our client. However, this isn't good enough as the nodes are hard-coded.

```javascript
//users.js
function showUsers() {
	const entry = document.getElementById('JSEntry');
	const n1 = document.createElement('p');
	const t1 = document.createTextNode(
		'awdeorio has 0 snippets'); // Note that this is hard-coding still
	n1.appendChild(t1); // text is a child of <p>
	entry.appendChild(n1); // <p> is a child of <div id="JSEntry">
	const n2 = document.createElement('p');
	const t2 = document.createTextNode(
		'jflinn has 0 snippets');
	n2.appendChild(t2);
	entry.appendChild(n2);
}
showUsers();
```

Now, suppose we had received some data about the users from REST API. To read the data structure into the DOM, we need to modify our codes here.

```javascript
function showUsers() {
	const entry = document.getElementById('JSEntry');
	const users = [
		{
			"snippets": [],
			"url": "/api/v1/users/100/",
			"username": "awdeorio"
		},
		{
			"snippets": [],
			"url": "/api/v1/users/200/",
			"username": "jflinn"
		}
	]; 
	// Note this is same as what we would get from a REST API response
	users.forEach((user) => {
		const n = document.createElement('p');
		const s = `${user.username} has ${user.snippets.length} snippets`;
		const t = document.createTextNode(s);
		n.appendChild(t);
		entry.appendChild(n);
	});
}
```

So far, these three approaches would've got us the same outcome.

Let's now, **really** play with the real data responded by the server.

#### Fetch API
The Fetch API provides an **interface** for HTTP requests. This is step 7 in our agenda above.
- A built-in function in JS (ES 6)
- Call a function when the response arrives
	- Parse `JSON` into JavaScript object
- Call another function when `JSON` parsing is finished
	- Add DOM nodes using JavaScript object

Why calling functions? This is related to *Asynchronous programming*, something we will cover in the next note.

From server's perspective, we are doing three things here.
```shell
127.0.0.1 - - [28/Sep/2017 18:28:30]
	"GET / HTTP/1.1" 200 -
127.0.0.1 - - [28/Sep/2017 18:28:30]
	"GET /static/users.js HTTP/1.1" 200 -
127.0.0.1 - - [28/Sep/2017 18:28:30]
	"GET /api/v1/users/ HTTP/1.1" 200 -
```
First, we make  `GET` requests for the HTML source code plus associated JS code. **THEN**, we make another `GET` request to the **REST API** endpoint as made by the **FETCH API**.

In our previous example, our `showUsers()` function would look something like this.

```javascript
function showUsers() {
	const entry = document.getElementById('JSEntry');
	
	function handleResponse(response) {
		if (!response.ok) throw Error(response.statusText);
		return response.json(); 
	} // This is just a definition, not run until called

	function handleError(error) {
		console.log(error);
	}

	function handleData(data) {
		const users = data.results;
		users.forEach((user) => {
			const node = document.createElement('p');
			const text = `${user.username} has ${user.snippets.length} 
							snippets`;
			const textnode = document.createTextNode(text);
			node.appendChild(textnode);
			entry.appendChild(node);
		});
	}
	
	fetch("/api/v1/users/") 
		.then(handleResponse) 
		.then(handleData)
		.catch(handleError);
	// Output of first function used as input to the second function
}
```

^1f7ef4

#### Closure
Notice that JavaScript allows **inner functions** in the previous example!
- This is because functions are first class objects in JavaScript, and we can create them **at runtime**

Also notice that the **inner function** has access to **outer function**'s variables
- E.g. `const entry`
- Lexically scoped name binding
	- First look at the inner scope and see if the variable exists, if not we go to the outer scope

This is called a **closure**.
- The inner function has a **longer lifetime** than the outer function
- A **class** is **DATA** with **functions** attached
- A **closure** is a **FUNCTION** with **data** attached

In addition, the `entry` variable used by the inner function `handleData()` is **NOT** a copy, it is a **reference** to the original object created by the outer function `showUsers()`.
- Possible, again because of closure, as the inner function has the **context** in which it was created, the scope of the outer function (the activation record)

![[Pasted image 20230930163655.png|400]]
It is counter-intuitive that a stack frame or an activation record is popped off when a function returns; however, in JavaScript, a stack frame is really just a **pointer** to a doubly linked-list of **activation records**.
- Activation records only get deleted when no longer deleted (i.e. no longer referenced by a closure)
- Activation records **really** live on the **heap**, but acts like a stack

##### Closure in Interpreter
Let's sketch this out using our previous example.
1. Objects created for `entry`, `handleResponse`, `handleData`
2. Fetch function executes
1. Enqueue callbacks `handleResponse`, `HandleData`
2. Fetch returns **before** response arrives
3. Stack pointer points to null
4. .... Wait for response
5. Later, response arrives and `handleResponse` executes
6. Later, JSON data is ready and `handleData` executes

Closures are important because **callback functions** are everywhere, and they need to know their context.

---
### Anonymous Function
Note that in the [[#^1f7ef4|previous example]], the callback functions, `handleResponse(response)` and `handleData(data)` are only called **ONCE**. In JS, we can refactor them using what's called the anonymous functions:

```javascript
function showUsers() {
	const entry = document.getElementById('JSEntry');
	/* ... */
	fetch('/api/v1/users/')
		.then(function(response) {
			//...
		})
		.then(function(data) {
			//...
		})
}
```

They work the same as when the functions had names and are also called **lambda function** or function literal.

ES6 provides a convenient syntax for anonymous functions, using the *arrow functions*:

```javascript
function showUsers() {
	const entry = document.getElementById('JSEntry');
	/* ... */
	fetch('/api/v1/users/')
		.then((response) => {
		//...
		})
		.then((data) => {
		//...
		})
}
```

An anonymous function is consisted of three parts:
- Inputs
- Body
- Arrow

