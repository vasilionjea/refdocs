# Getting started with Vim

## Interactive tutorials
`vimtutor` is an interactive tutorial covering the most basic aspects of text editing. On UNIX-like system, you can start the tutorial with:

    $ vimtutor

Other interactive tutorials include these browser-based ones:
- [Vim Adventures](https://vim-adventures.com/) – An interactive game version of vimtutor available on the web. Only the first few levels are free.
- [Open Vim](http://www.openvim.com/) – An interactive terminal which teaches you the basic commands with feedback.
- [Vim Genius](http://www.vimgenius.com/) – Another interactive terminal which also includes intermediate and advanced lessons including macros and arglist.


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

## Exiting Vim
In order to exit Vim, first make sure you are in *Normal* mode by pressing <kbd>Esc</kbd>.

 - `:q` <kbd>Enter</kbd> (will prevent you from exiting if you have unsaved changes - short for :quit)

To *discard* changes and exit Vim:

 - `:q!` <kbd>Enter</kbd> to force exit and discard changes (short for `:quit!`, not to be confused with `:!q`),
 - `ZQ` is a shortcut that does the same as `:q!`,
 - `:cq` <kbd>Enter</kbd> quit and return error (discard all changes so the compiler will not recompile this file)

To *save* changes and exit Vim:

 - `:wq` <kbd>Enter</kbd> (shorthand for `:write` and `:quit`),
 - `:x` <kbd>Enter</kbd> (same as `:wq`, but will not write if the file was not changed),
 - `ZZ` is a shortcut that does the same as `:x` (Save workspace and quit the editor),
 - `:[range]wq!` <kbd>Enter</kbd> (write the lines in [range])

To close multiple buffers at once (even in multiple windows and/or tabs), append the letter `a` to any of the *Commands* above (the ones starting with `:`). For example, to write and quit all windows you can use:

  - `:wqa` <kbd>Enter</kbd> or
  - `:xa` <kbd>Enter</kbd> &mdash; Write all changed buffers and exit Vim.  If there are buffers without a file name, which are readonly or which cannot be written for another reason, Vim will not quit
  - `:xa!` <kbd>Enter</kbd> &mdash; Write all changed buffers, even the ones that are readonly, and exit Vim.  If there are buffers without a file name or which cannot be written for another reason, Vim will not quit
  - `:qa` <kbd>Enter</kbd> &mdash; try to quit, but stop if there are any unsaved files;
  - `:qa!` <kbd>Enter</kbd> &mdash; quit *without saving* (discard changes in *any* unsaved files)

If you have opened Vim without specifying a file and you want to save that file before exiting, you will receive `E32: No file name` message. You can save your file and quit using:

 - `:wq filename` <kbd>Enter</kbd> or;
 - `:x filename` <kbd>Enter</kbd>

#### Explanation:

The <kbd>:</kbd> keystroke actually opens *Command* mode. The command `q` is an abbreviation of `quit`, `w`, of `write` and `x`, of `exit` (you can also type `:quit`, `:write` and `:exit` if you want). Shortcuts _not_ starting with `:` such as `ZZ` and `ZQ` refer to *Normal* mode key mappings. You can think of them as shortcuts.

The `!` keystroke is sometimes used at the end of a command to force its execution, which allows to discard changes in the case of `:q!`.
Placing the `!` at the beginning of the command has a different meaning.
For example, one can mistype `:!q` instead of `:q!` and vim would terminate with a 127 error.

An easy way to remember this is to think of `!` as a way of insisting on executing something.
Just like when you write: "I want to quit!"


## Suspending Vim
When using `vim` from the command line, you can suspend `vim` and get back to your prompt, without actually quitting `vim`. Hence you will later be able to get back your `vim` session from the same prompt.

When in *Normal mode* (if not, press <kbd>esc</kbd> to get there), issue either of these commands:

> `:st`<kbd>enter</kbd>
>
> `:sus`<kbd>enter</kbd>
>
> `:stop`<kbd>enter</kbd>
>
> `:suspend`<kbd>enter</kbd>


Alternatively, on some systems, when in *Normal* or *Visual* mode, issuing <kbd>Ctrl</kbd>+<kbd>Z</kbd> will have the same effect.

**Note:** If `autowrite` is set, buffers with changes and filenames will be written out. Add a `!` before <kbd>enter</kbd> to avoid, eg. `:st!`<kbd>enter</kbd>.

Later, when you want to return to your `vim` session, if you haven't suspended any other jobs, issuing the following will restore vim as your foreground job.

> `fg`<kbd>enter</kbd>

Otherwise you will need to find your `vim` sessions's job ID by issuing `jobs`<kbd>enter</kbd> and then foregrounding the matching jobs `fg %[job ID]`<kbd>enter</kbd> eg. `fg %1`<kbd>enter</kbd>.

  
_Source: <https://stackoverflow.com/documentation/vim/879/getting-started-with-vim>_
