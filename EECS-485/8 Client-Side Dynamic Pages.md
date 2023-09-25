[[2023-09-24]] #Webpage #JavaScript

### JavaScript
- Python with C++ syntax, AKA ECMA Script
- Only programming language web browsers support
- Interpreted

#### `null` and `undefined`
- `null`: a value that indicates a **deliberate** non-value
	- By assignment
- `undefined`: a value of type undefined that indicates an uninitialized value (usually "**accidental**")

```
> let x;
undefined
> x === undefined;
true
```

#### Primitives
Primitives and objects are JavaScript's abstraction for data. 
- Use `let` to define; sometimes we can also use `const`
- `string`, `boolean`, `null`, `number`, `undefined`, `symbol`
Primitives are **NOT** objects **and** have **NO** **methods**.
- Note the lowercase
- Immutable literals

#### Objects
Objects have **properties** and a **prototype**.
- "Key-value pairs"
- E.g. `{ name:"Web systems", num: 485 };`

Properties are **values** associated with an **object**.
- Named
- Unordered

#### Data Model: Built-in Objects
Primitive values have **object equivalents** that **wrap** around the primitive values.
- `String`, `Number`, `Boolean`, `Symbol`, `Array`, `Map`, `Set`
	- Except `null` and `undefined`
- Has useful member functions

#### Type System
JS is **dynamically** typed.
- Language extensions add static type checking with a compiler: Flow, TypeScript

#### Prototypes
Prototypes are the mechanism by which JavaScript objects **inherit features** from one another.
- No distinction between **instances** and **classes**
- **VERY different** from inheritance in other languages!

Every JS object has a prototype attribute.
- Akin to the object’s “parent”
- All objects inherit the **properties and methods** from their prototype
- When resolving a reference, JS climbs prototype tree until name is found (or not)
- The prototype is **ANOTHER object**, not a superclass
- Examine it via the `__proto__` attribute
- CAREFUL (also: WEIRD): `__proto__` and `prototype` are not the same thing!

#### Pitfalls
##### Equality Operators
JavaScript has two sets of equality operators: `===` and `!==`, and their Evil twins `==` and `!=`.
- `==` performs a **type conversion** when comparing two things 
	- `'' == '0' // false`
	- `0 == '' // true`
	- `0 == '0' // true`
- `===` performs **NO** type conversion, returns `false` if types differ

```ad-warning
Always use `===` and `!==`! In this class we will rely on `eslint`.
```

##### Scope 
- Simply **assigning** values always creates a **global variable**.
- `var` creates a local or global scoped variable
	- Functions create scope - thus local variable
	- Other blocks like `if` do not - thus global variable
- `let` and `const` create **block**-scoped variables
	- A lot like C/C++

```ad-warning
**NEVER** use `var`. Always use `let` and `const`.
```

##### Hoisting
Variables declared with `var` are **hoisted** to the **top** of the function.
- Thus this works: 

```javascript
> function f() {
	console.log(x === undefined);
	var x = 5;
}
> f();
true
```

Whereas variables declared with `let` or `const` are **NOT**.

```javascript
> function f() {
	console.log(x === undefined);
	let x = 5;
}
> f();
ReferenceError: x is not defined
```

##### `const`
`const` means you can't **reassign** the **reference**.

```javascript
> const eecs485 = { name: 'Web Systems', num: 485 };
> eecs485 = { name: 'Chicken Stories', num: 101 };
TypeError: Assignment to constant variable.
```

However, changing the **object** is OK.

```javascript
> const eecs485 = { name: 'Web Systems', num: 485 };
> eecs485.name = 'Chicken Stories';
'Chicken Stories'
> eecs485.num = 101;
101
> eecs485
{ name: 'Chicken Stories', num: 101 }
```

In reality `const x` in JS is really like `int *const p` in C.

##### F

---
### Review: Web Pages
- Static pages
	- Responds with same content every time the page is loaded
	- Only HTML + CSS
	- **Client**: browsers are HTML renderers
	- **Server**: HTTP fileservers
	- E.g. `python3 -m http.server`, Nginx, Apache
- Server-side dynamic pages
	- Response is the output of a **function**
		- Client makes a request, server executes a function and outputs (usually an HTML) the response
	- Generation of content is **specific to each request**

```ad-info
There are limitations, obviously. Since server-side dynamic pages are created at time of request, they don't change after the HTML has been generated and transferred to client's browser.

That means
- No browser chat
- No browser-based field validation
- No grabbale map
```

---
### Client-Side Dynamic Pages
In short, JavaScript running in the client’s web browser modifies the DOM. The rendered page changes.

We can break this down into three steps:
1. Client requests **static** **HTML** page from server
	- Source file often contains a `<script src="script.js">` tag
2. Client requests** static JavaScript** file from server
3. Client executes JavaScript
	- Using a JavaScript interpreter built in the browser
4. JavaScript code modifies the DOM
	- Document Object Model: tree data structure in the memory of the browser, representing what's **ON** the page
	- Finds a node in the DOM, then changes the node or add or remove a node, etc.
5. Rendered page changes

```ad-important
In order to achieve client-side dynamic pages, **NO** programming language is needed.
```

Let's demonstrate this with an example.

```HTML
<!-- index.html -->
<!DOCTYPE html>
<html>
<body>
	<!-- the code for hello() is in script.js -->
	<button onclick="hello()">
		Click me!
	</button>
	<div id="JSEntry"></div>
	
	<!-- script tags go in body, not in head -->
	<script src="script.js"></script>
</body>
</html>
```

```javascript
//script.js
function hello() {
	console.log("Hello World!");
	n = document.getElementById("JSEntry"); // Find a node in DOM
	n.innerHTML = "Hello world!";  // Change the text
}
```

```ad-example
1. In the HTML snippet, note that the `hello()` function is defined in a JavaScript file, which is included in the `<script>` tag.
2. The event name in the `button` **MUST match** with the function name in JavaScript.
3. In the JavaScript file, note how the argument in `getElementById()` relates to the `id` of one of the `div`.

![[Pasted image 20230924180315.png|500]]
```

We can also put the JavaScript  in the `<script>` block rather than in a separate file.
#### DOM
Let's continue with the previous example of `hello()`. Again, the DOM is a tree data structure. 

The browser gets the following when it makes the initial `GET` request. Then, it finds the `script` tag and thus makes its second `GET` request for the script.
```tree
html
	- body
		- button
			- text: Click me!
		- div id=JSEntry
		- script
```

Upon clicking the button, this event makes JS function run. It looks for the `div` with the corresponding `id` and adds a text `node` to it.

```tree
html
	- body
		- button
			- text: Click me!
		- div id=JSEntry
			- text: Hello world!
		- script
```

#### `console` API
A global API that runs text to the developer console.
- Code running in the browser's interpreter has access to it
- A `print` statement that can be used in debugging

![[Pasted image 20230924181511.png|500]]

#### `document` API
Represents the web page loaded in the browser.
- Web page represented as a DOM
- Essentially why JS code can modify what the user sees
	- JS code can **change** elements in the DOM, thus the page

---
### Event-driven Programming
In event-driven programming, the flow of the program is determined by events.
• A few examples of events built into the browser:
	• `onclick`: user clicks a button
	• `onmouseover`: The user moves the mouse over an HTML element
	• `onkeydown`: The user pushes a keyboard key
	• `onload`: The browser has finished loading the page

Event-driven programming is useful for Graphical User Interfaces (GUIS) like **web applications**.

#### Callback Functions
An infinite main loop (in C code of the JavasCript interpreter) listens for events and triggers a **callback function**.
- A callback function is just a normal function, **waiting** to be executed
	- Waiting for the `onclick` event
- Current example: `hello()` is a callback

#### Event Handlers
In the HTML, we registered our function as an **event handler**.
- That means telling the browser "please run this function when X event occurs"

How is this achieved? The JavaScript interpreter maintains a **table of events** that **map** to functions. Example:

| Events    | Function |
| --------- | -------- |
| `onClick` | `hello`         |

#### Event Queue
In JavaScript, **function** calls live on the stack, **objects** live on the **heap**, and **messages** live on the **queue**
- The function on the top of the stack executes
- When the stack is empty, a **message** is **taken out** of the queue and processed
- Each message is a function
- An event adds a message to the queue

```ad-important
A program never ends in JS. Remember what happens to callback function?
```

Let's demonstrate this process with an example.
```javascript
function callback1() {
	console.log('this is a msg from callback1');
}
setTimeout(callback1, 1000);
```

![[Pasted image 20230924184855.png|500]]
Initially, the stack is empty (global frame doesn't count) and we see our function (an object) defined in the heap which is referred in the global frame. Nothing in the event table or queue yet.

![[Pasted image 20230924185033.png|500]]

Then, the function is added to the event table as a **reference** to the function. The event is defined to be happening in one second. How do we know this? The main loop keeps on checking the condition for us.

![[Pasted image 20230924185202.png|500]]

After 1000 ms, the event entry is popped and we added a corresponding message function in the queue, which also is a **reference** to the original function in the heap.

![[Pasted image 20230924185314.png|500]]

When the stack is empty, this message is taken out of the queue and **processed**. Now the function appears on the top of the stack and is executed, and the user will see the output in the console log. 