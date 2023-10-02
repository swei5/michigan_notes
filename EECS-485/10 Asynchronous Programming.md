[[2023-10-01]] #JavaScript 

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