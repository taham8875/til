# Vim keybinding

Vim keybinding is a set of keybinding that is inspired by vim, they are not exclusive to vim, they can be used in your favorite editor, like vscode.

# How to exit vim

To exit vim, you need to press colon `:` to enter command mode, then type `q` to quit.

- `:q` quit (will not quit if there are unsaved changes)
- `:q!` quit without saving
- `:wq` save and quit
- `:w` save

# How to move cursor vertically

When you enter vim, you are in the command mode by default, the mouse is pretty useless, you can navigate the file using the following keys:

- `h` move left
- `j` move down
- `k` move up
- `l` moves right

- `G` move to the end of the file
- `gg` move to the beginning of the file

- `}` move a block of code down
- `{` move a block up

- `ctrl + d` move half page down
- `ctrl + u` move half page up

- `:<line number>` to go to a specific line number, for example, `:10` will go to line 10

> In vim, don't memorize the keybinding, each key on the keyboard is a word, and these words make up a sentence, let's practice it:
>
> - `G{` move to the end of the file, then move a block up

# Insert mode

Now if you reached the place where you want to edit, you need to enter the insert mode, you can do that by pressing `i` in the command mode and start typing.

You can also enter the insert mode by pressing `o`, it will insert a new line below the current line and enter the insert mode. (press `O` to insert a new line above the current line)

- `i` enter the insert mode
- `o` insert a new line below the current line and enter the insert mode
- `O` insert a new line above the current line and enter the insert mode
- `a` move one character to the right and enter the insert mode
- `A` move to the end of the line and enter the insert mode
- `I` move to the beginning of the line and enter the insert mode

To exit the insert mode, you can press `esc` to go back to the command mode.

> Many people like to remap the caps lock key to esc and vice versa, since esc is used a lot and the caps lock position is very convenient.

# How to move horizontally

- `w` move to the beginning of the next word
- `b` move to the beginning of the previous word

- `W` move to the beginning of the next word (ignore punctuation)
- `B` move to the beginning of the previous word (ignore punctuation)

- `0` move to the beginning of the line
- `^` move to the first non-blank character of the line (equivalent is `_`)

- `$` move to the end of the line

> `0w` is equivalent to `^`

- `t<char>` move to the character before the next `<char>` (jump) (exclusive)
- `f<char>` move to the character after the next `<char>` (jump) (inclusive)

- `T<char>` move to the character before the previous `<char>` (jump) (exclusive)
- `F<char>` move to the character after the previous `<char>` (jump) (inclusive)

- `%` move to the matching bracket (parenthesis, square bracket, curly bracket)

# How to delete

- `dd` delete current line
- `D` delete to the end of the line

- `x` delete current character

> Practice:
>
> - `d}` delete the next block of code
> - combine the `d` with any movement keybinding you learned above to delete what you want

# Usage of numbers

Numbers are used to repeat the command (multiplier), for example, if you want to delete 3 lines, you can type `3dd`, if you want to move 5 lines down, you can type `5j`.

> by default, numbers column are hidden in vim, to enable it:
>
> - `:set number` show line numbers
> - `:set nonumber` hide line numbers
> - `:set relativenumber` show relative line numbers (recommended)

# Undo and redo

- `u` undo
- `ctrl + r` redo

# Period

Period is used to repeat the last command, for example, if you want to delete 3 lines, you can type `3dd`, if you want to delete another 3 lines, you can either type `3dd` again, or you can type `.`, which will repeat the last command.

# Copy and paste

- `y` copy
- `yy` copy current line
- `p` paste below current line
- `P` paste above current line

Note the `dd` command will copy the deleted line to the clipboard (cut), so you can paste it somewhere else.

# Visual mode

Visual mode is used to select text, you can enter visual mode by pressing `v` in the command line, then you can use the keybindings you learned above to interact with the selected text.

- `v` enter the visual mode
- `V` enter the visual line mode
- `ctrl + v` enter the visual block mode

# Change

- `c` change
- `cw` change word
- `cc` change line
- `C` change to the end of the line
- `ct<char>` change to the character before the next `<char>` (exclusive)

# Replace

- `r` replace current character with one character
- `R` replace current character with multiple characters

# Search

- `*` search the word under the cursor
- `;` search the next occurrence
- `,` search the previous occurrence
- `/` search

# Capitalization

- `~` capitalize the character under the cursor

# Indentation

- `>>` indent current line
- `<<` unindent current line

# Additional keybinding

- `zz` center the screen on the cursor

- `vi{` select the current block of code (without the curly brackets)
- `va{` select the current block of code (with the curly brackets)
(this also works for parenthesis and square brackets)
