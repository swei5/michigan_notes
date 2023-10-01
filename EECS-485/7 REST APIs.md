[[2023-09-23]] #RESTAPI #Webpage  
### HTTP Review
As seen in [[2 Static Pages|previous chapter in Static Pages]], HTTP request methods indicate server action.
- `GET`: request a resource
	- Load a page
- `HEAD`: identical to `GET`, without response body
	- See if page has changed (browser does this to save time re-downloading the same file)
- `POST`: send data to server
	- Web form

---
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
curl -s <path> | jsonlint
```

Trailing commas are **NOT allowed**.

#### `curl`
A command line utility that issues a **[[2 Static Pages#^12804f|get request]]** and returns a `JSON` formatted string.
- Even better, we can use `Httpie` to make the returned `JSON` object more colorful and readable
- We can test it via `http <request_type> httpbin.org/anything <JSON object>`

---
### REST API
- Not a REST API: **NOT** a human-**readable** text
	- HTML
- REST API: Machine-readable text ^b4608e
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

#### View
- Detail view or Item view
	- Returns one object from the database
	- `JSON` format returned in a typical response

```json
curl localhost:8000/<path>/<slug>/
{
	"key": "value",
	...
	"url": /<path>/<slug>/
}
```

Notice how the `url` verifies the address we're feeding and the **slug**/id (1 in this case) part of the URL, which helps us get the correct item from the database.

- List view or Collection view

```json
{
	"results": [
		{
			"key1": "value1",
			...
		}, 
		{
			"key2": "value2",
			...
		},
		...
	]
}
```

Each item in the `JSON` array is information about one  particular object.

#### Pagination
REST API enables **pagination**.
- Giving user *part* of data at a time, and let them request more data with a second request
- List/collection view is more useful in this case

On the backend, this is done using **query parameter**.  For instance, if we want to get 50 results from the page:
```bash
curl localhost:8000/<path>/<slug>/?size=50
```

#### Verbs and Status Code
Verbs are just HTTP methods.

In addition to `GET`, `HEAD`, and `POST` HTTP methods we've seen earlier, there are a few more verbs we need for REST API. In summary, in REST API
- `GET`: return datum
	- Response `404 NOT FOUND` on a non-existing file request
- `POST`: create new datum
	- Request includes `JSON` body. E.g.
		`POST <url> HTTP/1.0`
		`{"key": "val", ...}`
	- Response includes a **copy** of the created object and a **link to itself**. E.g.
		`{"key": "val", ..., "url": <url>/<slug>/}`
		- Returns `201 CREATED` on success
- `PATCH`: update part of datum
	- Request URL includes an ID and a `JSON` body. E.g.
		`PATCH <url>/<id> HTTP/1.0` 
		- `JSON` Only contains the field that should be **modified**
	- Response includes a **copy** of the **entire** modified object
		- Returns `200 OK` on success
- `PUT`: replace the entire datum
	- Request URL includes an ID and a `JSON` body
		- `JSON` contains a replacement value for every field
	- Response includes a **copy** of the **entire** modified object
		- Returns `200 OK` on success
- `DELETE`: delete datum
	- Request URL includes an ID, **NO body** however
	- Returns `204 NO CONTENT` and also **No body**

```ad-info
The reason why `PATCH`, `PUT`, and `DELETE` requires an ID in the URL is that they are associated with a specific object in the database, whereas post is operating on a **new** object.
```

To find out what status codes mean, check out  [this website]( https://restfulapi.net/http-status-codes/ ).

#### Design Principle
- Resource-based and manipulation of resources through representations
	- Individuals resources are identified in requests using **URIs** as resource identifiers.
		- URI like a pointer (ID in the URL)
			- E.g. `GET /api/v1/p/1/`
		- Returned object usually contains a link to itself 
			- E.g. `{..., "url": "api/v1/p/1/"}`
- Self-descriptive message
	- Describes to content how to process the message
		- E.g. `>Content-type:application/json; charset=utf-8`
- HATEOAS
	- Hypermedia as the Engine of Application State
	- Everything the server needs to know should be included in **ONE** request
	- Links are included in the returned object for retrieval of related objects
- Client-server architecture
	- **Abstraction** between client and server
		- Change the server without modifying the client
		- Change the client without modifying the server
- Stateless
	- Consistent with HTTP
	- Everything needed to handle the request is in the request itself
	- After server done processing, state, status, and response are communicated back to the client
- Cacheable
	- Clients cache some kind of responses to eliminate requests
	- Responses should implicitly or explicitly define themselves as **cacheable**
		- E.g. `Last-Modified` header
- Layered
	- A client **CANNOT** ordinarily tell whether it is connected directly to the end server or some intermediaries
		- Just need the URI