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
### AJAX
