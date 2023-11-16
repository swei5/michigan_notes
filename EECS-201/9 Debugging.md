[[2023-03-25]] #System 

### `printf` Debugging
Very Intuitive: just print stuff out at certain points.
- With the power of preprocessors, we can turn it on and off!

#### Logging
An extension on printing.
- Provide different verbosity/logging levels
- Log to **standard** **output**, **standard** **error**, or to some file

Common logging levels include
- Fatal: we can't continue
- Error: something went wrong
- Warning: something might be going on
- Debug: something happened, here's the details
- Trace: **EVERYTHING**

---

### GDB (GNU Debugger)
GDB is a debugging tool that lets you look around **DURING** **execution**.

We can invoke it by
```bash
gdb [options] [executable file] [core file]
```

Hitting return/enter without anything will **repeat** the previous command.

Also has an approximation of a windowing interface in "Text User Interface" (TUI) mode.
- `tui enable`

#### Commands
-  `run [arguments] [file redirects]`
- `next [count]`: step over
- `step [count]`: step into functions
- `finish`: step out of
- `print <expression>`: print expression
- `break <location>`: set breakpoint
- `watch <expression>`: set watchpoint

#### Breakpoints
Can be conditional!
- `info b` will list breakpoints
- `break main.cpp:21 if argc == 4`

#### Watchpoints
Stop when an expression **changes**.
- `info watch`
- `watch somevar`

To disable/delete a breakpoint or watchpoint, we can do
- `disable <number>`
- `delete <number>`

---

### `Valgrind`
General dynamic analysis tool.
- Most known for Memcheck tool for checking **memory accesses**
	- Memory leaks
	- Use-after-frees
	- Invalid reads
	- Use of uninitialized variables

`valgrind`  can seriously slow down the program, however.

To invoke, we use
```shell
valgrind --leak-check=full ./myapplication
```
