# Getting started with Vim
_Run interactive [vim tutorials][1] as many times as needed to feel comfortable with the basics._

## Basics
Vim features several modes, e.g. **normal mode**, **insert mode** and **command-line mode**.

**Normal mode** is for editing and navigating text. In this mode `h`, `j`, `k` and `l` correspond to the cursor keys `←`, `↓`, `↑` and `→`. Most commands in normal mode can be prefixed with a "count", e.g. `3j` moves down 3 lines.

**Insert mode** is for inserting the text directly, in this mode vim is similar to other more simple text editors. To enter insert mode press `i` in normal mode. To leave it press `<ESC>` (escape key).

**Command-line mode** is for running more complex commands like saving the file and exiting vim. Press `:` to start the command-line mode. To leave this mode you can also press `<ESC>`. To save the changes to the file use `:w` (or `:write`). To exit vim without saving your changes use `:q!` (or `:quit!`).


These are some of the more useful commands in vim: 

Command | Description
---|---
`i` | (insert) enters insert mode *before* the current cursor position
`I` | enters insert mode *before* the first printable character of the current line
`a` | (append) enters insert mode *after* the current cursor position
`A` | enters insert mode *after* the last printable character of the current line
`x` | delete character at the current cursor position
`X` | delete character at the left to the current cursor position
`w` | move to next word 
`b` | move to previous word
`0` | move to the beginning of line 
`$` | move to the end of line
`r` | replace – enters replace mode for one character. The next character you type will replace the character under the cursor.
`R` | enters replace mode indefinitely. Every character you type will replace the character under the cursor and advance the cursor by one.
`s` | substitute – deletes the character at the current cursor position and then enters insert mode
`S` | delete the current line that the cursor is currently on and enter insert mode
`<Esc>`, `<C-c>` | exit insert mode and returns to normal mode
`u` | undo
`<C-r>` | redo
`dd`, `dw`, `dl`, `d$` | cut the current line, from the cursor to next word, or the character, current position to end of current line respectively, note: `D` is the equivalent of `d$`
`cc`, `cw`, `cl` | change the current line, from the cursor to next word, or the character, respectively
`yy`, `yw`, `yl`, `y$` | yank ("copy") the current line, from the cursor to next word, or the character, current position to end of current line respectively
`p`, `P` | put ("paste") after, or before current position, respectively
`o`, `O` | to create a new empty line, after or before the current one and enter insert mode
`:w` | write the current buffer to disk
`:q!`, `ZQ` | quit without writing
`:x`, `:wq`, `ZZ` | write and quit
`:help` | open a window with help file
`:help {subject}` | show help for a specific subject
`qz` | begin recording actions to register `z`, `q` to end recording, `@z` to play back the actions. `z` can be any letter: `q` is often used for convenience.


  [1]: http://stackoverflow.com/documentation/vim/879/introduction-to-vim/5715/built-in-vim-tutorial-vimtutor#t=201610260327193388628
  
_Source: <https://stackoverflow.com/documentation/vim/879/getting-started-with-vim>_
