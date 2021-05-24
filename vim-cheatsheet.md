# The humble Vim cheat sheet

Basic Vim commands I keep forgetting.

## File I/O

### Open files

|                                            |                  |                                                  |
| ------------------------------------------ | ---------------- | ------------------------------------------------ |
| Open existing file in a new buffer         | `:e <file>`      | Support wildcards, e.g. `**/<file>`              |
| Open new file in a horizontal split window | `:new`           |                                                  |
| Open new file in a vertical split window   | `:vnew`          |                                                  |
| Open new file in the current window        | `:enew`          |                                                  |
| Open new file in a new tab                 | `:tabnew`        |                                                  |
| Read file content                          | `:r[ead] <file>` | Insert text content of `<file>` below the cursor |

### Save files

|                               |                       |      |
| ----------------------------- | --------------------- | ---- |
| Save current file             | `:w`                  |      |
| Save all files                | `:wa`                 |      |
| Save current file as new file | `:saveas <file-name>` |      |

## Buffers operations

|                       |                 |      |
| --------------------- | --------------- | ---- |
| List open buffers     | `:ls`           |      |
| Close current buffer  | `:bd[elete]`    |      |
| Go to next buffer     | `:bn[ext]`      |      |
| Go to previous buffer | `:bp[previous]` |      |

## Copy and paste

|                              |         |      |
| ---------------------------- | ------- | ---- |
| Copy the current line        | `yy`    |      |
| Copy `n` lines               | `<n>yy` |      |
| Paste below the current line | `p`     |      |
| Paste above the current line | `P`     |      |

## Screen layout

### Windows

|                            |                            |      |
| -------------------------- | -------------------------- | ---- |
| Split window horizontally  | `:split`                   |      |
| Split window vertically    | `:vsplit`                  |      |
| Move cursor across windows | `Ctrl-w + <direction key>` |      |
| Close current window       | `Ctrl-w + q`               |      |

### Tabs

|                              |                              |                                                              |
| ---------------------------- | ---------------------------- | ------------------------------------------------------------ |
| Create new empty tab         | `:tabn[ew]`                  |                                                              |
| Create new tab with a buffer | `:tabe[dit] <file or path>`  |                                                              |
| Move tab                     | `:tabm[ove] <numeric value>` | e.g. `:tabm -1`, `:tabm +3` (relative), `:tabm 2` (absolute) |

## Text editing

|                                                     |      |      |
| --------------------------------------------------- | ---- | ---- |
| Insert mode                                         | `i`  |      |
| Jump to next line and start editing                 | `o`  |      |
| Jump to the beginning of the line and start editing | `I`  |      |
| Jump to the end of the line and start editing       | `A`  |      |

### Indentation, tabs and spaces

|                          |                       |                                                              |
| ------------------------ | --------------------- | ------------------------------------------------------------ |
| Insert tabs as spaces    | `:set expandtab`      |                                                              |
| Set tabs-as-spaces width | `:set shiftwidth=<x>` | Where `x` is the number of spaces to insert. Works only if `:set expandtab` is on |
| Insert tabs as real tabs | `:set expandtab!`     | Returns to the default behavior                              |

### Deletion

|                                            |      |                                       |
| ------------------------------------------ | ---- | ------------------------------------- |
| Delete line                                | `dd` |                                       |
| Clear line from cursor to end              | `D`  |                                       |
| Clear whole line                           | `0D` | `0` moves the cursor to the beginning |
| Clear whole line and end up in insert mode | `S`  |                                       |

## Text selection

|                                               |                           |                                                              |
| --------------------------------------------- | ------------------------- | ------------------------------------------------------------ |
| Visual mode (character)                       | `v`                       |                                                              |
| Visual block (blocks)                         | `Ctrl-v`                  |                                                              |
| Visual line (lines)                           | `V`                       |                                                              |
| Virtual mode                                  | `:set virtualedit=<mode>` | Cursor can move where there is no actual character. `<mode>` can be: `block` for visual block mode, `insert` for insert mode, `all` for all modes, `onemore` allows the cursor to move just past the end of the line |
| Select current word under cursor              | `viw`                     |                                                              |
| Select everything inside parentheses          | `vi[parenthesis]`         | e.g. `vi(`, `vi[`, `vi{`                                     |
| Select everything inside block (curly braces) | `viB`                     | Same as `vi{`                                                |

## Text navigation

|                                               |      |      |
| --------------------------------------------- | ---- | ---- |
| Move to beginning of line                     | `0`  |      |
| Move to end of line                           | `$`  |      |
| Move to first non-blank character of the line | `_`  |      |
| Move to last non-blank character of the line  | `g_` |      |
| Move to first line                            | `gg` |      |
| Move to last line                             | `G`  |      |
| Jump between the last two positions           | `''` |      |

### Marks

|                        |                       |                                                              |
| ---------------------- | --------------------- | ------------------------------------------------------------ |
| Set local mark         | `m<lowercase-letter>` | Sets a mark in the current file, e.g. `ma`. Available letters: `a-z` |
| Set global mark        | `m<uppercase-letter>` | Sets a unique, global mark in the current file, e.g. `mA`. Only one file can have a specific mark. Available letters: `A-Z` |
| Go to mark             | `'<letter>`           | E.g. `'a`, `'B`                                              |
| Delete mark            | `:delmarks <letter>`  |                                                              |
| Delete all local marks | `:delmarks!`          |                                                              |

## Text appearance

|                                  |                        |                                                              |
| -------------------------------- | ---------------------- | ------------------------------------------------------------ |
| Word wrap                        | `:set wrap`            |                                                              |
| Break lines at words             | `:set linebreak`       |                                                              |
| Highlight current line           | `:set cursorline`      |                                                              |
| Show vertical line               | `:set colorcolumn=<x>` | Where `<x>` is the column number                             |
| Show all whitespaces             | `:set list`            |                                                              |
| Automatic word wrapping          | `:set textwidth=<x>`   | Text wraps automatically as you type, in order to make each line long `<x>` characters at most |
| Show existing tabs as `x` spaces | `:set tabstop=<x>`     |                                                              |

## File browser

Vim has a built-in file browser called **netrw**.

|                                                    |               |      |
| -------------------------------------------------- | ------------- | ---- |
| Open file browser in the current window            | `:Ex[plore]`  |      |
| Open file browser in a new split horizontal window | `:Sex[plore]` |      |
| Open file browser in a new split vertical window   | `:Vex[plore]` |      |
| Open file browser in a new tab                     | `:Tex[plore]` |      |

### Inside the file browser

|                          |      |                                           |
| ------------------------ | ---- | ----------------------------------------- |
| Change view mode         | `i`  | Switch between normal, compact list, tree |
| Create a new directory   | `d`  |                                           |
| Create a new file        | `%`  |                                           |
| Delete file or directory | `D`  |                                           |
| Rename file or directory | `R`  |                                           |

## Running external commands

|                                                        |                         |                                                              |
| ------------------------------------------------------ | ----------------------- | ------------------------------------------------------------ |
| Run external command                                   | `:! <command> %`        | E.g. `:! wc %`: runs the `wc` command on the current file (`%`) and reports the results |
| Re-run last command                                    | `:!!`                   |                                                              |
| Run external command and save output to current buffer | `:r[ead] ! <command> %` |                                                              |