[[2023-03-08]] #System

### Overview
- Programs
	- Sequence of instructions to perform
	- Typical computers speak **binary** ("machine code")
		- x86-64
		- aarch64
	- **Compiled** high-level languages get turned into this binary form
		- C++, Java
	- **Interpreted** high-level languages are interpreted by another program
		- Shell scripts, Python

#### Building Programs
Traditional compiled programs have multiple steps to produce an executable.
- **Source code**: **human** readable code in a (high-level) language
- **Object code**: chunk of **CPU-understandable** machine code
	- File formats include additional metadata for tools to deal with
- **Executable**: fully put together ("linked") chunks of machine code
	- Ready for the operating system to load and run as a new process

There are 3 steps in building a program.
1. **Compiling**: turn high-level code into lower-level code
	- High-level source to lower-level "high-level" language (e.g. Java to C)
	- High-level source to assembly
	- High-level source to object code
2. **Assembling**: turn low-level (assembly) code into object code
3. **Linking**: putting object code together into something usable
	- Produces executables: has a starting point (e.g. `main`), has resolved dangling references
	- Produces libraries: code that other programs can call on

---

### Build Systems
In `gcc`, building an executable goes something like `gcc -o app file1. c file2. c file3.c`.

Build system is a tool/system to **automate** building software.
- Compiling
- Packaging
- Testing

A simple build system example using script could look something like:
```bash
#!/bin/bash  
gcc -o myapp src/file1.c src/file2.c src/file3.c src/main.c
```

Better, still:
```bash
#!/bin/bash  
gcc -o myapp $(find src -name "*.c")
```

However, this can be a bit of work to custom write a script, especially with larger projects.
- Will blindly compile everything, every time
	- Imagine having to recompile everything every time I made a small code change
		- We can just build the object code for that **modified file**, then **link** the object code
		- Shell can't do this

---

### GNU `make`
- Classic tool that helps with build automation.
- Provides more abstractions over a plain shell script
- Invoke by running `make`
- Will look for a `Makefile` to run

#### `make` Rule and Structure
The `Makefile` will specify rules that have **prerequisites** that have to be met/built before running its recipe to build a target file.

```cmake
target: prerequisites  
	recipe
```

- The `target` is assumed to be some **actual file** that gets produced
- `make` is able tell if the built `target` file is **newer** than **prerequisite** files to avoid unnecessarily performing the recipe
	- `prerequisite` targets will be built **before** the target's recipe will be able to be run
- Recipe consists of shell commands
- `make <target>` builds a specific `target`

A concrete example looks like
```cmake
myapp: src/file1.c src/file2.c src/file3.c src/main.c  
	gcc -o myapp src/file1.c src/file2.c src/file3.c src/main.c
```

The overall idea is to have rules that depend on other rules that build **smaller bits** of the system.
- Allows for incrementally building our project
- Invaluable with enormous code bases

#### Phony targets
The `.PHONY` target can specify phony targets that don't have actual files.
- `all`
- `clean`
- `test`

This is done by specifying
`.PHONY: all clean test`

#### Variables
We can define variables in Makefiles as well (we can put spaces around the ' `=` '! this time).

```ad-important
There are two flavors of variable expansions, however.
- `varA = $(varB)` **recursively** **expands** the right hand side whenever the variable gets expanded
	- If `varB ` gets reassigned after this, an expanded `varA` will expand the **CURRENT value** of `varB`
	- "`varA` 's value is **whatever** `varB` 's is"
- `varA := $(varB)` performs a **simple expansion**, taking on the value of the right hand side **at assignment time**
	- "`varA` 's value is "some-value""
```

---

#### Automatic Variables
In a rule, `Make` has some automatically assigned variables.
- `$@`: target's file name
- `$<`: First prerequisite
- `$?`: All prerequisites newer than the target
- `$^`: All prerequisites
- ...

#### Functions
There are some functions that can be expanded to help with **text manipulation**.
- `$(wildcard <pattern>)` can expand to a file list for files that match the pattern
	- `$(wildcard *.c)`
- `$(shell <commands>` can run a shell command and expand to the command's output
	- `$(shell find . -name "*.c")`
- `$(<var>:<pattern>=<replacement>)`, known as a substitution reference, can perform replacements
	- `$(SOURCES:%.c=%.o)`

Combining what we've learned so far:
```cmake
CC := gcc  
BIN := myapp  
SRCS := $(shell find src -name *.c) 
$(BIN): $(SRCS)
	$(CC) -o $@ $^
```

#### Pattern Matching
We use `%` to match file names. For instance, the file below compiles `.c` files into `.o` files

```cmake
%.o : %.c  
	$(CC) -c -o $@ $<
```

