[[2023-02-09]] #RegularExpressions

### Regex
A **pattern** that matches a set of strings.
- Provide a (relatively) **standardized** way to perform **matches** on text
- Need **tools**/**utilities** to make use of them
	- `grep`, `sed`, `find`, etc.
	- grep is a utility that **searches** for patterns in a file or input via **regexes**
	- `grep` could serve as an offline tester for regex

Patterns are composed of **smaller** **regexes** that are **concatenated**.
- `hello` is a regex that matches the string "hello"
- `h`, `e`, `l`, `l`, `o` are each **atomic regex** that concatenate to form the overall regex

---

### Regex Basics
There are also special functions denoted by **special** **characters**.
- `.` for any single character
	- `...` matches three consecutive characters
- `|` for an OR
	- `hello|world` matches a string that is "hello" or "world"
- `\`  for special expressions/escapes
	- `\b` matches the empty string at the "edge" of a word
- Quantifiers: **how** **many** to match
- Brackets: a **set** of **characters** to match
	- `(hello|hi) (John/Joe)` matches:
		- "hello John"
		- "hello Joe"
		- "hi John"
		- "hi Joe"
- Anchors: for *positional* matching
- Backreferences: for matching a **previous** match

---

### Quantifiers
Specify **how many** of a **preceding** **regex** to match.
- `?`: $\le 1$ time
- `*`: $\ge0$ times
- `+`: $\ge1$ times
	- `ba+` matches "ba", "baa", etc.
- $\{n\}$: $n$ times
	- `a{4}` matches "aaaa"
	- `(hello){3}` matches "hellohellohello"
- $\{n,\}$: $\ge n$ times
- $\{,m\}$: $\le m$ times
- $\{n,m\}$: $x$ times where $n\le x\le m$

---

### Brackets
- `[,]` enclose a set to match for **one character**
	- `[abc]` matches 'a', 'b', or 'c'

Special things we can put inside them:
- `-`: range
	- `[A-Za-z0-9]`: capital and lowercase numbers and digits
- `^`: **NOT** in set
	- `[^ab]`: everything not 'a' or 'b'
- Named classes
	- `[:alnum:]`: alphanumeric characters
	- `[:alpha:]`: alphabetic characters
	- `[:space:]`: space and tab characters

```ad-warning
The named classes **GO INTO** the brackets too!
- e.g. `[[:alnum:]]` for alphanumerics
```

---

### Anchors
Perform positional matching.
- `^`: match empty string at the **beginning** of a line
	- following regex must be at the beginning
	- `^hello`: "hello" must be at the beginning
- `$`: match empty string at the **end** of a line
	- preceding regex must be at the end
	- `world$`: "world" must be at the end

```ad-example
`^hello world$`: entire line **MUST** be "hello world"
	- `hello` should be able to match against the "hello"
	- `^hello$` wouldn't be able to match because "hello" is not at the end
```

---

### Backreferences
Match previous parenthesized `()` subexpression.
- `\n`: match $n$ th **parenthesized** subexpression
	- `(123)testing\1` matches "123testing123"