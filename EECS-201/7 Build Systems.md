[[2023-03-08]] #System #Make

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
---  
build.sh  
---  
#!/bin/bash  
gcc -o myapp src/file1.c src/file2.c src/file3.c src/main.c
```

