#Unix #Shell
### \*nix

```ad-important
**Definition 1.1**:
"\*nix" refers to a group of operating systems either derived from or inspired by the original AT&T Unix from **Bell Labs**.
- **GNU/Linux** is a "Unix-like"  
- mac OS is an actual Unix derivative  
- \*nix systems follow similar principles and provide similar (software) interfaces
```

Unix and its derivatives have entrenched themselves in academia and industry  
- The many tools developed to run on *nix systems are mature and are here to stay
- General \*nix literacy will help you since you have a pretty good likelihood to be developing on a \*nix system

This does not mean that \*nix systems are inherently better than other operating systems like Windows.

---

### Command Line Basics

```ad-important
**Definition 1.2**: The "command line" is a type of interface where you provide a line of text that the interpreting soware can interpret into commands to perform.
```

This is known as a **shell**.
- There are also *graphical* shells, like Finder.

#### Why Terminal?
- Before we had graphical displays we printers and teletypes (TTYs)
	- Then, we moved to **video terminals** that emulate an actual terminal
- Unix and the many tools for it were developed during these times
- Text serves as a long lasting, reliable interface that is very easy to automate
	- Graphical GUIs change and update frequently and is hard to automate

#### Shells
The \*nix command line is known as the *shell*.
- Follows very similar basic syntax no matter what shell (bash, zsh, csh, etc.) you use

Follows the structure: ^a03f80

![[Pasted image 20230119003258.png|600]] ^a049ce

---

#### \*nix and the Filesystem
\*nix exposes everything as a file.
- We address and locate files via *paths*
- Each running program (including the shell) has a "current working directory"
	- `/` enters, separates directories
	- `.` refers to the **current** directory
	- `..` refers to the **parent** directory

```ad-note
There are two types of paths.
- Absolute: start with `/`
	- we call `/` the **root directory** - the starting point
		- `/home/brandon/Music/deemo-saika-rabpit.flac`

- Relative: starts from current or parent directory
	- `./dir1/dir2`
	- `../../some-dir`
	- **NOTE**: Implicitly starts from **current directory** if path doesn't start with `/`, `.`, or `..`.
```

^612acf

---

#### Extra Commands
- `man`: "manual pages": gives info on programs
- `mv`: "move": moves files to another directory (actual moving) or another filename (renaming)
- `grep`: searches files for data matches

Lots of commands **act** on files.
- `nano some-file.txt`
- `vim some-code.cpp`

#### Output
- we can pipe output from command to another command with a pipe (`|`)
	- `echo "hello world" | rev`
	- Effect: prints the reverse of `hello world`
- We can save output from a command to a file with a "redirection" (`>`)
	- `echo "hello world" > some-file`
- We can retrieve input from a file for a command with another "redirection" (`<`) ^0e8068
	- `amongus -TSP < test-f`

---

### Automation
We can save a list of commands into a file.
- Known as the *script*

We can now run this script whenever we want by **invoking** the filename as an **argument** for shell of choice
- `$ zsh some-script`

This runs a **new shell instance** that runs each of those commands as if you had entered in the commands yourself.

If the file is marked as **executable**, you can also directly invoke it as a **program**.
- `$ ./some-script`
- **HAVE** to specify it as an **explicit path**