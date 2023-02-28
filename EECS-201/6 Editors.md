[[2023-02-27]] #Editors

### Editor History
- `ed` (1969)
	- Original UNIX editor
	- POSIX spec
- `vi` (1976)
	- Modal text editor with *command*, *insert*, and *command-line* modes
	- ESC brings you Command mode
- `vim` (1991)
	- Superset of `vi`
- `emacs` (1976, 1984)
	- Highly extensible
	- Exit with `C-x C-c` where `C-` is **control**
- `nano` (2000)
	- Typical basic text editor
	- `^G` for more shortcuts
	- `^X` to exit
- Sublime Text (2008)
- VS Code (2015)

#### `vi`
- Basic controls
	- `<ESC>`: Enter command/normal mode
	- `h`, `j`, `k`, `l`: mover cursor left, down, up, right
	- `w`: to the beginning of next word
	- `b`: to the back of current word
	- `e`: to the end of current word
	- `0`: to the beginning of line
	- `$`: to the end of line
	- `/`: to search for pattern
- Editing
	- `i`: to insert
	- `a`: to append
	- `x`: to delete characters under cursor
	- `r`: to replace characters under cursor
	- `u`: to undo
	- `^r`: to redo (`vim` only)
- Command-line/ `ex` mode
	- `:e` : to edit
	- `:w [file_name]`: to write/save to a particular file
	- `:q`: to quit
	- `:q!`: to quit without saving
	- `:x`: to quit, write if modified