[[2023-10-01]] #JavaScript #Webpage 

### Asynchronous Programming, Introduction
Asynchronous programming is tasks interleaved with one another, in a **single thread** of control.
- Programmer controls when tasks "take turns"
- **NOT** a single-thread blocking program (queue-like structure, tasks wait until previous executed)
- **NOT** a multi-thread blocking program (modern OS)

Examples of tasks include `fetch()`, `json()`, and responding to user clicking a button on
UI and update UI.

![[Pasted image 20231001151047.png|400]]

#### Why Asynchronous?
TL; DR: To eliminate the waiting time (grey spots in the previous graph)!  
- UIs: by interleaving the tasks, system is **responsive** to user input while **still performing** other work in the "background”
- Waiting for I/O: do other "useful things" while **waiting for I/O**, like a network or disk
	- Useful things like
		- Respond to user mouse hover event
		- Respond to user clicking a radio button
		- Check for new mail (Gmail)
	- Synchronous programs are bad at this

We want to use asynchronous programming when
- There are **a large number** of tasks so there is likely **ALWAYS** **at least ONE** task that can make progress
- The tasks perform **lots of I/O**, causing a synchronous program to waste lots of time blocking when other tasks could be running
- The tasks are largely **independent** from one another so there is little need for inter-task communication
	- Thus no need for one task to **wait** for another

These conditions are common in web systems!
- Twisted: a networking library in Python
- NGINX: a web server
- AJAX: Asynchronous JavaScript and XML (JSON)

---
### JavaScript Event Revisit
In JavaScript, function calls live on the stack, objects live on the heap, and messages live on the queue.
- Like other programming languages, the function **on the top** of the stack executes.
- An event adds a message to the queue
	- Each message is a function

```ad-important
Asynchronous programming exists **ONLY** when the stack is empty, a **message** is taken out of the queue and processed.

If, on the other hand, our JS function only has vanilla functions (doesn't involve any callbacks, which means no functions are parameters of some other functions), then we don't have to deal with asynchronous programming.
- Functions just get added to the stack when they are called, same as C++/Python
```

Let's show this with a more complicated example. Assume we have the following snippets of code.

```javascript
function callback1() {
	console.log('this is a msg from callback1');
}
setTimeout(callback1, 1000);
slow();
```

As we previously learned, a **[[8 Client-Side Dynamic Pages#^74c441|callback function]]** only will get executed when the stack pointer points to **NOTHING** on the heap, meaning that the outer function has returned.
- This is equivalent to saying that the [[8 Client-Side Dynamic Pages#^f6f8ef|event loop]] will only start looking at the event table and event queue if there is nothing on the stack

---
### AJAX
AJAX stands for Asynchronous JavaScript and XML.
- XML is a misnomer these days, we use JSON

We have already coded an AJAX app from the previous notes!

```javascript
function showUser() {
	function handleResponse(response) {/*... */}
	
	function handleData(data) {/*...*/}
	
	fetch('https://api.github.com/users/awdeorio')
	.then(handleResponse)
	.then(handleData)
}
showUser();
```

Here, `handleResponse()` runs **asynchronously**, after server response arrives. Also, `handleData()` runs **asynchronously**, after JSON parsing is finished.

Here's a diagram that denotes this process:

![[Pasted image 20231001232957.png|500]]

Now let's add an extra function that shows a pop-up when user hovers over some text. Now, what happens if the user hovers before the server response arrives?
- User still sees the pop-up - thanks to asynchronous operations

![[Pasted image 20231001233430.png|500]]

---
### Promises
Promises control the **flow of deferred** and **asynchronous operations**. They are first class representation of a value that may be made asynchronously and be **available** in the future.
- Examples of values that will be available in the future include
	- The response to a server request: `fetch()`
	- The data from parsing a JSON string: `json()`
- In simple terms, this is an object to which you can attach a callback using `.then()`

Let's discuss this using the example of `fetch()`. `fetch()` returns a `Promise`. After the value is available, the `Promise` calls a function provided by `.then()`.

```javascript
function showUser() {
	fetch('https://api.github.com/users/awdeorio')
		.then((response) => {
			return response.json();
		})
	.then((data) => {
		console.log(data);
		})
}
```

In general, functions performing **asynchronous tasks** return a `Promise`.

#### Promise States
A `Promise` is in one of these states:
- **Pending**: initial state, neither fulfilled nor rejected
- **Fulfilled**: meaning that the operation completed successfully
- **Rejected**: meaning that the operation failed

#### Chaining Promises
A common need is to execute two or more asynchronous operations back-to-back, where each **subsequent operation starts** when the **previous operation succeeds**, with the result from the previous step.
- Request
- Parse JSON

We can accomplish this using a **promise chain**, where the output (resolved value) of one `Promise` is the input to the next. Again, this is precisely what we saw in the example above.

![[Pasted image 20231002001738.png|600]]

```ad-tldr
The function executes **IMMEDIATELY** (each returning a `Promise`), and the callback functions execute later, asynchronously (after the value of the `Promise` is made available).
```

We can also provide a callback for handling a errors. A Promise will call one of the two callbacks provided by
- `.then()`
- `.catch()`

A promise chain stops if there's an **exception**, looking down the chain for **catch handlers** instead.
- REST API returned `4xx` will trigger error
- Similar to try/catch in synchronous code

```javascript
function showUser() {
	fetch('https://api.github.com/users/awdeorio_has_chickens')
		.then((response) => {
			if (!response.ok) throw Error(response.statusText);
			return response.json();
		})
		.then((data) => {
			console.log(data);
		})
		.catch((errorResponse) => {
			errorResponse.json()
				.then(errorData => {
					console.log('Add these errors to UI:', errorData);
			});
		})
}
```

---
### Creating Promises
Next, we'll look at them from the perspective of creating a `Promise` "from scratch".

For instance, if we have the following code snippet:

```javascript
function callback1() {
	console.log("1 second passed");
}
setTimeout(callback1, 1000);
```

We can refactor it to use a `Promise`.

```javascript
function wait(ms) {
	return new Promise(resolve => {
		setTimeout(resolve, ms);
	}); // this is equivalent to CONSTRUCTING a Promise object
}
```

A `Promise` has an executor function. An executor function normally initiates some asynchronous work, and Calls `resolve()` once the work **completes**.
- In this example, this is `setTimeout(resolve(), ms)`

A Promise allows you to associate **handlers** with an asynchronous action's eventual **success value**.
- In this example, this is `resolve`, or what's defined in `.then()`: could be a function or anonymous function

A Promise allows you to associate **handlers** with an asynchronous action's eventual **failure reason**.
- This is what's in `.catch()`: could be a function or anonymous function

```ad-note
**Relation to synchronous methods**

Asynchronous methods (like `wait`, `fetch`) return values like synchronous methods. However, instead of **immediately returning** the final value, the asynchronous method **returns** a `Promise` to supply the value at some point in the future.

```

---
### Asynchronous vs. Event-driven
Asynchronous programming **describes** the execution. 

Event-driven describes the **implementation**.

```javascript
// somewhere in the JS interpreter
while (queue.waitForMessage()) {
	queue.processNextMessage();
}
```

#### ES8
Includes `async` and `await` keywords. Also provides a syntactic sugar for a `Promise`, `.then()`!
- `async` functions return a `Promise`
- `async` functions can contain `await` expressions
- `await` pauses the execution of the `async` function and waits for the passed `Promise` 's resolution, and then resumes the `async` function's execution and returns the resolved value.

This is basically the workflow that we talked about in this entire note. See this example, again

```javascript
// ES8 async/await style
async function showUser() {
	// await response of fetch call
	let response = await fetch('https://api.github.com/users/awdeorio');
	// only proceed once promise is resolved
	let data = await response.JSON();
	// only proceed once second promise is resolved
	// add nodes to DOM here
	console.log(data);`
}
```