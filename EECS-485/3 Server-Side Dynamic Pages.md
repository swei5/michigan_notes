[[2023-05-07]] #Webpage #Python

### Dynamic Pages
In general, static pages stay the same and dynamic pages can change.
- **Static** if file is read from disk verbatim
	- Content is **SAME** every time
- **Dynamic** if a program generates the page only when client asks for it
	- Content changes, e.g. Web search, Database lookup
	- Each **request** calls a **function** in the server software, which generates the **response**

The most common pattern includes
- Store data in a database
	- Then, use `jinja`/templates to create HTML from that data

In this way, users can change the data.

![[Pasted image 20230508194522.png|450]]

---

### Dynamic Server
Let's first show an example using Flask.

```python
import flask
app = flask.Flask(__name__)

@app.route("/hello/")  # this is pointing to http://localhost/hello
def hello_world():
	return "<html><body>Hello World!</body></html>"

if __name__ == "__main__":
	app.run()
```

Flask is a micro web framework written in Python, which allows us to create dynamic pages using a Python program.

---

#### Templating HTML
Sometimes, we'd love to have templated HTML files too for the sake of comprehensiveness.
- E.g. Python's `jinja2` library to write the template
	- Then, run it through a rendering function, `flask.render_template()`, for instance

A simple HTML file containing `jinja2` syntax could look something like
```html
<html>
<head><title>Hello world</title></head>
<body>

<!-- jinja syntax starts -->
{% for word in words %}
{{word}}
{% endfor %}
<!-- jinja syntax ends -->

</body>
</html>
```

After the program is done rendering, this would be equivalent to
```html
<html>
<head><title>Hello world</title></head>
<body>

hello

world

</body>
</html>
```

How does the above get done dynamically? We need to refine our program. To work with `flask`, we need to store our **variable** `words` used by `jinja` ("hello" and "world") in this case, and provide this value as an argument in the rendering function:

```python
@app.route("/hello/")
def hello_world():
	context = {"words": ["Hello", "World!"]} # used by jinja
	return flask.render_template("hello.html", context)
```

```ad-important
We may be prompted to think that data and computation are separate, but they are really two sides of the same coin.
- Data instead of computation
	- Is really pre-computing and storing results
- Computation instead of data
	- Is really dynamically generating web pages
```

---

#### URL Routing
Dynamic pages are created by **executing a function** at the time of a **request**. 

```ad-question
- When a client requests a URL, how does a server know **which function** to call?
- What about routes like a user page, where the function could have an **input**?
```

We could use a **routing table** to display how a request is tied to a route/function.
| Route      | Function           |
| ---------- | ------------------ |
| `/u/user1` | `show_user(user1)` |
| `/explore` | `show_explore()`   |
| `/p/`      | `show_post()`                   |

The implementation again goes back to another `flask` function. 

```python
# creates a route
@app.route('/users/<username>/') # <username> is a placeholder for argument
def show_user(username):
    return "hello {}!".format(username)

@app.route('/posts/<postid>/')
def show_post(postid):
    return "post {}!".format(postid)
```

### Python Decorator
In Python, functions are **first class** objects
- Can be passed as input
- Can be returned as output
- Created/destroyed at run time

A **decorator** is a **function** that takes **another function** and extends the behavior of the latter function without explicitly modifying it.

For example,

```python
@wrapper
def f():
	print("Hello")
```

is equivalent to

```python
def f():
	print("Hello")
f = wrapper(f)
```