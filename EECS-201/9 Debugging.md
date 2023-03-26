[[2023-03-25]] #Debugging #System 

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

### GDB (GNU Debugger)
GDB is a debugging tool that lets you look around **DURING** **execution**.

We can invoke it by
```bash
gdb [options] [executable file] [core file]
```
