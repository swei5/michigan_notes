[[2023-02-03]] #Shell #System

### Shells
Interactive shells are the shell that you directly **interact** with at a **terminal**.
- zsh, bash
	- zsh is backwards compatible with bash but with more features
- Can run scripts with **different interpreters** but personalize your working environment
- `sh` is a **POSIX** standard

Recall how shell commands are formulated: ![[3 Unix#^fcd64b]]

---

### Shell Operation, Job Control
- See [[3 Unix#^31bd4d|here]] for basic operation rules in a shell.
- See [[3 Unix#^7373d7|here]] for how/where to find programs to execute in a shell.

```ad-important
Two very important commands.
- `^C (SIGINT)` can cause most process to stop
- `^Z (SIGTSTP)` can cause most processes to suspend
```

-  `jobs` can list out **processes** (jobs table) that the shell is managing.
- `bg` can **background** a process, yielding the terminal **back** to the shell
- `fg` can **foreground** a process, giving it active **control** of the terminal
- `disown` can have the shell **give up ownership** of a **process**
- The `?` variable holds the **exit status** of the **last command**

- See [[3 Unix#^3643a3|here]] for the usage of variables/environmental variables in a shell.
- See [[3 Unix#^c9934d|here]] for our previous knowledge in **file redirection**.

#### More on File Redirection
-  `<<`: "Here document"; given a **delimiter**, enter **data** as **standard input**
- (Bash) `<<<`: "Here string"; provide **string** directly as **standard input**
	- The two commands below do the same thing:
		- `echo "some string" | rev`
		- `rev <<< "some string"`
- `[n]<>` set file as input and output on fd `n`
	- Default is fd 0
	- `3<>file`
- (Bash) `&>`: set files as fd 1 and fd 2, overwrite (`stdout` and `stderr` go to same file) 
- (Bash) `&>>` : set files as fd 1 and fd 2, append (`stdout` and `stderr` go to same file) 

#### Grouping
We can group commands together as a unit.
- `(commands)` performs commands in a *sub-shell* - another **shell process/instance**.
- `{ commands; }` performs commands in the **calling shell instance**

```ad-warning
There has to be spaces around the brackets and a semicolon (or newline or `&`) terminating the commands
```

#### Parameter Expansion
`$varname` will expand to the **value** of `varname`.
-  `${varname}`: we can use curly brackets to explicitly draw the **boundaries** on the variable name
- The `[]` means that contents are **optional**
	- `${varname:-[value]}` : use default value
	- `${varname:=[value]}` : assign default value
	- See manuals for more

#### Filename Expansion
- `*` matches any string
- `?` matches any single character
- `[...]` matches **one** of **any** of the characters enclosed in the brackets

A token/word with these will **expand** out to matching **filenames**.
- `*` expands to all the files in the current directory
- `file?.txt` expands to all files that start with "file", have a **single** **character**, then end in `.txt`

#### Command Substitution
`$(command)` will substitute the **output** of a command in the brackets.
- `$(echo hello | rev)` will be substituted with "olleh"
	- The command in the command substitution will be **run** first to get the output, then **substituted**
- Can also do **arithmetic expansion** on `$((expr))` - works for integers only, however

#### Quoting
Allows you to **retain** certain characters without Bash **expanding** them and keep them one string.
- Single quotes (') preserves **ALL** of the characters between them
- Double quotes (") preserve **ALL** characters **except**: `$`, `\`, and backtick
	- Variables expanded inside of double quotes **retain** their white-space
	- `ls "home/directory with spaces"` works
	- Or, `ls "home/directory\ with\ spaces"` with the escape chars

---

### Commands

#### Conditionals
You can use any commands for conditions, but these constructs should be familiar:
- `test expr`: `test` command
	- Shorthand is `[ expr ]` - mind the **whitespaces**
		- `test $a -eq $b` is equivalent to
		- `[ $a -eq $b ]`

- `||`, `&&`, just like what we see in C
- `!` the **NOT** operator

```ad-warning
When we chain condition operators together, we must also mind the whitespaces!
- `$a || ! $b`
- **NOT** `$a ||! $b`
```

##### Bash Conditionals
- `[[ expr ]]`
	- Provides a richer set of operators: `==`, `=`, `!=`, `<`, `>`, etc.
	- `-lt` if we are comparing two **numeric values**, since our inputs are strings
- `((expr))`
	- Evaluates **arithmetic** expression


#### Controls
What a `while` loop looks like:
```shell
while test-commands; do 
	commands
done
```

Its reverse:
```shell
until test-commands; do 
	commands
done
```

The `until` loop repeats commands until the condition **succeeds**.

What a `for` loop looks like:
```shell
for var in list; do 
	commands
done
```

If `list` is not present, it will implicitly iterate over the **argument** **list**.
- `$@`
- `1 2 3 4 5`
- `$(ls)`

What a switch looks like:
```shell
case value in  
	pattern1 ) commands1 ;;  
	pattern2 ) commands2 ;;  
	multpat1 | multpat2 ) commands3 ;; 
	* ) commands
esac
```

Value is **matched** against **patterns**.
- When a pattern is matched its **command**(-list) is **run**
- A **wildcard** (`*`) pattern is often used to represent a "default" case

What a function looks like:
```shell
function hello-world () {
	if echo "Hello world!"; then 
		echo "This should print"
	fi
}
```

A compound command is a **command group** (`()`, `{}`), or a control flow (`if-else`).
- Called by invoking them like any other **utility**, including passing **arguments**
	- `$n` - `n` is the argument number
	- `$@` - list of arguments
	- `$#` - number of arguments

To call the function above, simply put
```shell
hello-world
```

---

### Shell Scripts
Recall what we have learned before about [[3 Unix#^7373d7|executables]] and [[3 Unix#^2e964f|scripts]].

There's a few nuances.
- `./my-script`: tells the **OS** to execute the `my-script` file
	- The OS will try to **identify** the file and will look for a **[[3 Unix#^fd875b|shebang]]** for the **interpreter**
- `bash ./my-script` tells the **OS** to execute `bash` with `my-script` file as an argument
	- It's up to `bash` to figure out what to do with the file

#### Running VS Sourcing
- **Running** (executing) a script puts it into its **own shell** **instance**; shell variables set **WON'T** be **visible** to the parent shell.
- **Sourcing** a script makes your **current shell instance** run each command in it; shell variables set will be visible
	- Essentially how to set up the python `venv`

---

### Configuring Shell
Shells will automatically source certain files to perform configuration.
- `/etc/profile`: system-wide configuration
- Can make own additions to `~/.zshrc`
	- `export PATH="newdir:$PATH"`
	- Add alias for directories
	- Add prompts
		- The `PS1` and `PS2` variables hold the prompt information

#### Terminals
- `ctrl-l`: clear screen
- `reset`: reset the terminal
- `ctrl-d`: send EOF - no more inputs for commands, and closes **CLEANLY**