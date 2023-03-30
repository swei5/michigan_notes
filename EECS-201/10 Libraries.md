[[2023-03-30]] #Library #System 

### Libraries
- Collections of code and data that can be used by other programs
	- GUI: `libxcb`, `libX11`
	- Graphics: `libvulkan`
	- File formats: `libpng`, `libjpeg`

C and C++ libraries happen to serve the backbone of a **complete OS**.

There are three types of libraries.

---

#### Source Libraries
- Source code for a library is provided
- Pretty much exactly like a normal project

To use source libraries, just add more source code.

---

#### Static Libraries
- Provided as an **archive** of **pre-compiled** object code
	- Named as `lib<name>.a`
		- `.a` stands for *archive*
- Tossed into the executable
- Will not change
	- Large size cost, however, as the library is part of the executable

#### Dynamic/Shared Libraries
- A collection of object code meant to be shared by multiple programs
	- Named as `lib<name>.so`
		- `.so` stands for *shared object*
		- `.dylib` and `.dll` are macOS and Windows counterparts
- Executable is **linked** against this library
- Library is marked as a **dependency**
	- Resolved at program load time
	- Avoids static linking size cost

To use static/dynamic libraries, we use the `-l<name>` linker flag.
- This searches through `/lib` and `/usr/lib`
- Doesn't need to specify static or dynamic
- Can specify additional directories with `-L`
	- For instance,  `-lm` for `libm.a` and `libm.so`

```bash
gcc -o myapp $(SRCS) -lm
gcc -o myapp $(SRCS) -Lsomedir -lstaticlib
```

##### Conflicts
`.so` has a higher priority over `.a`.
- To use `.a`, we can do `-l:libm.a` to specify that we are interested in using static lib

---

#### Creating Static Libraries
- Compile
	- `gcc -c -o somecode.o somecode.c`
		- `-c`: compile but **DON'T link**, produces an object file
- Archive
	- `ar rcs libmylib.a somecode.o morecode.o yaycode.o`
		- `ar`: an archive tool
		- `r`: command, insert files with replacement
		- `c`: option, *create the archive*
		- `s`: option, *write an object file index into the archive*


#### Creating Dynamic Libraries
- Compile
	- `gcc -c -fPIC -o somecode.o somecode.c`
		- `-fPIC`: compile as **position** **independent** code
- Link
	- `gcc -shared -o libmylib.so somecode.o morecode.o yaycode.o`
		- `-shared`: produce a **shared object**
