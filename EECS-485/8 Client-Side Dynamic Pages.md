[[2023-09-24]] #Webpage #JavaScript

### JavaScript
- Python with C++ syntax, AKA ECMA Script
- Only programming language web browsers support
- Interpreted

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
