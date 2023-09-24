[[2023-09-23]] #RESTAPI #Webpage  
### HTTP Review
As seen in [[2 Static Pages|previous chapter in Static Pages]], HTTP request methods indicate server action.
- `GET`: request a resource
	- Load a page
- `HEAD`: identical to `GET`, without response body
	- See if page has changed (browser does this to save time re-downloading the same file)
- `POST`: send data to server
	- Web form

### REST API
- Not a REST API: **NOT** a human-**readable** text
	- HTML
- REST API: Machine-readable text
	- Representational State Transfer - a collection of **principles** and **practices **
	- Some form of data structure (`JSON`, for example)
	- Common use of REST API:
		 1. Javascript runs in browser
		 2. Javascript receives `JSON` data from server
		 3. JavaScript renders data on page
	- Commonly Use HTTP
		- Difference from normal HTTP response is that we get a different content type such as `JSON` instead of `HTML`
	- Great for interoperability between web systems
		- E.g. Laptop running JavaScript code and Instagram server

### `JSON` Data
Stands for JavaScript Object Notation.
- Commonly used to send data from server to web client

#### Structure
- Object: a collection of name/value pairs
	- `dict` in python and `map` in C++ (restrictions on types)
- Array: an ordered list of values
	- `list`
- Value:
	- String, number, boolean, `null`, **object**, **array**

To validate `JSON`, we can use one of `curl` 's utility:
```zsh
curl -s <url_to_check> | jsonlint
```

Trailing commas are **NOT allowed**.

#### `curl`
A command line utility that issues a **[[2 Static Pages#^12804f|get request]]** and returns a `JSON` formatted string.
- Even better, we can use `Httpie` to make the returned ``