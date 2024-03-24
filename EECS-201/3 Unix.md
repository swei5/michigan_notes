[[2023-01-24]]  #Unix #System #Shell 

### POSIX
A neat set of standards in industry and academia for Unix.
- C POSIX API: `unistd.h`, `fcntl.h`
- CL utilities: `cd`, `ls`
- Environments: `USER`, `HOME`, `PATH`

```ad-note
One of Unix's philosophies is to write programs to handle text streams, because that is a universal interface.
```

---

### Unix Component

#### Kernel
Software that serves as the **intermediary** between **hardware** resources and user **applications**.
- Manages hardware
- Handles multi-tasking, security environment, file systems
- Present a stable **application programming interface** (API) for user programs to use in the form of **system calls**

#### Library
- Reusable pre-written software you can call upon
- Provide functionality

#### Applications
- Software that users run and interact with or assist in the background
- Include bash, VS Code, `ls`, etc

These form an overall operating system.

---

### Unix Design
- Effectively boils down to processes **interacting** with **files**
	- Program: list of **instructions** to execute
	- Process: a running **instance** of a program
- Files serve as a sort of **universal interface**
	- Processes pass data to each other via a read/write interface

#### Unix Processes
- Identified by a process ID (PID)
- Associated with a **user**
- Has a current **working directory**
- Has an associated **program** image: the actual CPU instructions to run
- Has memory for **variables**
	- `PATH`
	- `PWD`
	- `USER`
	- `HOME`: user's home directory

- File descriptor table
	- Handles to various resources that have a file interface (read/write/seek)
	- File descriptors are indices into this table
		- `0`: Standard input (`stdin`, `cin`)
		- `1`: Standard output (`stdout`, `cout`)
		- `2`: Standard error (`stderr`, `cerr`)
		- POSIX functions: `open()`, `close()`, `read()`

#### Signals
A way to **communicate** with processes.
- `kill` signals processes
- `^C` sends SIGNIT (interrupt)
- `^Z` sends SIGTSTP (terminal stop)

For more references, see `man signal 7`.

---

#### Process Creation
A process calls `fork()` to make a **copy** of itself.
- The process is called the "parent" and the copy is called the "child"
	- The child is a **perfect copy** of the parent, except for the `fork()` return value, which is `0` for the child
- The child can call `exec()` to load a new program.
	- This **wipes** the process's memory for the **new program's data**
	- Environment variables and file descriptor table is left the same
	- Effectively, "loads" a new program

#### Unix Files
In Unix, ever everything is a file.

```ad-important
Unix files represent a **stream of bytes** that you can read from or write to.
- Serves as a neat interface: writing to a terminal is no different than writing to a file
- `stdin` and `stdout` are seen as files by your program
```

File name extensions have **NO** **intrinsic** **bearing** on the data.
- Most file formats have "magic numbers" in the first few bytes to help identify the file format
- Mostly help you organize/recognize files

Files have several properties.
- You can check them with `ls -l`
- `stat` can give more detailed information
	- `r`: read
	- `w`:write
	- `x`: execute
	- These three commands are often grouped together to form an octal digit
- `chmod` and `chown` can modify these

See this [Wikipedia Article](https://zh.wikipedia.org/wiki/Chmod)for more on file properties.

---

#### Links
A special kind of file is a "symbolic" or "**soft**" link.
- This file simply contains a **file** **path** to another file, kind of like a **shortcut**
	- `python3` can be symbolically linked to a concrete file named `python3.10`
	- The utility `ln` is able to create symbolic links with the `-s` flag

There are "**hard**" links as well.
- A hard link is really a **file path** to some underlying data
- A file starts off with one hard link: `the name/path` it was given

```ad-summary
- A hard link is directly linked to the underlying file data.
- A soft link says "go check this other file path for the data".
```

#### Unix Structure
- `/`: root, the beginning of all things
- `/bin`: binaries
- `/lib`: libraries
- `/etc`: configuration files
- `/var`: "variable" files, logs and other files that change over time
- `/home`: user home directories
- `/dev`: device files
- `/proc`: files that represent runtime OS information

In summary, it's just **processes** interacting with other **processes** and **files**.
- This leads us to **shell**

---

### Shell
Shell is simply a program like any other that receives input and produces output.
- Interpret its input as "create more **processes**"

Lots of shells floating around:
- Thompson shell, Mashey shell, Bourne shell
- The Bourne shell (`sh`) ended up becoming a de facto gold standard
- POSIX standard shell (`sh`) takes a lot from the Bourne shell

There are many shells around that build o of the POSIX shell (with more features):
- `bash`: the "Bourne again shell"
- `zsh`: the Z shell

Recall some basic shell operation structure: ![[EECS-201/1 Intro#^a049ce]] ^fcd64b

#### General Shell Operation

^31bd4d
1. Receive a **command** from a file or terminal input ^a40b51
	- `ls -l $HOME > some_file`
2. Splits it into **tokens** separated by **white-space**
3. Expands/substitutes special tokens
	- `$HOME` is equivalent to `/home/user_name` in this case
4. Perform file redirections ^31a3e5
	- Standard output to `some_file`
5. Execute commands ^f66b7b
	- `exec()`
	- `argc = 3`
	- `argv = ["ls", "-l", "/home/user_name"]`

We can also string commands together.
- `cmd1 && cmd2`
	- Run `cmd2` if `cmd1` **succeeded**
- `cmd1 || cmd2`
	- Run `cmd2` if `cmd1` **failed**
- `cmd1 ; cmd2`
	- Run `cmd2` **after** `cmd1`
- `cmd1 | cmd2`
	- Connect standard **output** of `cmd1` to **input** of `cmd2`
	- `echo "hello" | rev` prints `olleh`

```ad-note
A note on file redirection that `>>` sets file as standard output, **appends** (fd 1), whereas `>` **overwrites**.

The general form follows that:
- `[n]<` and `[n]>`, where `n` is optional fd (file descriptor)
	- e.g. `2>` captures `stderr` to a file
```

^c9934d

---

#### Environmental Variables

^3643a3

Shell **variables** are stored inside the shell **process**.
- Stored data in the process's memory
- Launched commands don't inherit them

Set them with `varname=varvalue`.

```ad-warning
**Whitespace** are important! `varname = varvalue` is interpreted as run `varname` with arguments `=, varvalue`.
```

You can set environment variables with `export`.
- `export varname=varvalue`
- `export existing_variable`
	- This sets a variable to be **exported** to **new** **processes**

##### Using Variables
You can use a variable with `$varname` or `${varname}`.
- The shell does is "expand" this to its **value**

---

#### Executing Programs

^7373d7

- If the command has a `/` in it, it's treated as a **filepath** and the **file** will be **executed**.
	- `$ somedir/somescript`
	- Only works if the file has its **execute** **bit** set

- If the command doesn't have a `/`, `PATH` will be searched for a corresponding **binary** (`/bin`)
	- `$ vim -> searches PATH` and finds it at `/usr/bin/vim`
	- This is why we want to specify `./` to run something

```ad-important
Some commands are "**built-in**"/implemented by the shell.
- These will take precedent over ones in the `PATH
```

There are  two classes of executable program. ^5e5cf2
- Binaries
	- Machine code (0/1s)
	- Various kinds of formats: ELF, Mach-O, PE, etc.
	- The first few bytes of these files usually have some **special byte sequence** to identify the file **type**

- Interpreted programs/scripts
	- Contains **human readable (plain) text** that map to some programming language
	- Typically run through an **interpreter** to do tasks
		- Python scripts, shell scripts ^2d4a35

##### Script

^2e964f

The first line of a script should contain a **shebang**. ^fd875b
- Tells the OS **what** **program** to use as an interpreter
- Starts with `#!` with the **path to the interpreting program** right after
	- `#!/bin/sh`: Run this script with sh
	- `#!/usr/bin/env python3`: Run this script with whatever `env` finds as `python3`

If there is no **shebang**, the OS assumes `sh`.
- It's usually a good idea to go with `sh` for universal **compatibility**

Arguments are passed in as **special variables**.
- `$n` : Argument `n`, note that `$0` will refer to the script's name
- `$@`: List of all arguments
- `$#`: Number of arguments

```ad-warning
There's a nuance between `$ ./my-script` and `$ bash my-script`.
- `$ ./my-script` tells the OS to execute the `my-script` file and will look for the shebang for the interpreter
- `$ bash my-script` tells the OS to execute `bash` with `my-script`
```
