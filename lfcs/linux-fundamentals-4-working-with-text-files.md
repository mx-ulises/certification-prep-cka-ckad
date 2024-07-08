## Using vim text editor

Working in linux happend in the command line, and you need a command line editor. Examples are:

 * `nano`: Is easier to use, but not avaialble everywhere
 * `vim` and `vi`: They are more powerful and more difficult to use and are available in all unix systems.

When you start `vim` it opens in command mode. The `vimtutor` provide instruction on how to use `vim` commands. To enter command mode press `Esc`. Some commands are:

| Command         | Description                                                                                              |
|-----------------|----------------------------------------------------------------------------------------------------------|
| `i`             | Switches to insert mode at the cursor position. You can start typing text from this point onward.        |
| `o`             | Opens a new line below the current line and switches to insert mode.                                     |
| `O`             | Opens a new line above the current line and switches to insert mode.                                     |
| `:wq!`          | Saves the current file and exits Vim, forcibly overriding any existing file without prompting.           |
| `u`             | Undoes the last action. Repeatedly pressing `u` continues to undo previous actions.                      |
| `Ctrl-r`        | Redoes the last undone action. This is the opposite of `u` (undo).                                       |
| `dd`            | Deletes the entire current line and moves it to the unnamed register.                                    |
| `3dd`           | Deletes three lines starting from the current line.                                                      |
| `d`             | Deletes the selected text in visual mode or the specified range (e.g., `dw` deletes a word).             |
| `dw`            | Deletes from the cursor position to the end of the current word.                                         |
| `y`             | Copies (yanks) the selected text in visual mode or the specified range (e.g., `yw` yanks a word).        |
| `:%s/old/new/g` | Performs a global substitution, replacing all instances of "old" with "new" in the entire file.          |
| `:q!`           | Quits Vim without saving changes to the file, forcibly discarding any unsaved changes.                   |
| `v`             | Enters visual mode, allowing you to select text character by character for further operations.           |
| `w`             | Moves the cursor to the beginning of the next word.                                                      |
| `b`             | Moves the cursor to the beginning of the previous word.                                                  |
| `^`             | Moves the cursor to the first non-blank character of the current line.                                   |
| `$`             | Moves the cursor to the end of the current line.                                                         |
| `/text`         | Searches forward in the file for the string "text". Use `n` to jump to the next occurrence.              |
| `gg`            | Moves the cursor to the beginning of the file.                                                           |
| `G`             | Moves the cursor to the end of the file.                                                                 |
| `:se number`    | Displays line numbers in the file. To turn off, use `:se nonumber`.                                       |
| `r`             | Replaces the character under the cursor with the next character typed.                                   |

## Commands to read files

The command `more` help you to page a file in pages that fit a screen. The command `less` offer more advances features including go to the previous screen using arrow screan. To change pages you can use the `spacebar` key.

| Command  | Description                                                                 |
|----------|-----------------------------------------------------------------------------|
| `more`   | Command-line pager that allows viewing text files one screen at a time, scrolling through the content. It stops after each screenful of text and waits for a keypress before proceeding. |
| `less`   | Command-line pager similar to `more` but with more advanced features like backward navigation and search functionality within the text. It allows scrolling both forward and backward through the text. |
| `less -f`| Forces `less` to open the file in "plain text" mode, disabling scrolling and thus behaving more like `more`, stopping after each screenful of text. It does not allow backward navigation or advanced features of `less`. |

The command `tail` shows the last few lines of a file. The command `head` shows the first few lines of a file. You can see things comming in realtime with the flag `-f`.

| Command    | Description                                                                                                         |
|------------|---------------------------------------------------------------------------------------------------------------------|
| `tail`     | Outputs the last 10 lines of one or more files or piped data.                                                      |
| `tail -3`  | Outputs the last 3 lines of one or more files or piped data.                                                        |
| `tail -f`  | Outputs appended data as the file grows (`follow`).                                                                |
| `head`     | Outputs the first 10 lines of one or more files or piped data.                                                      |
| `head -3`  | Outputs the first 3 lines of one or more files or piped data.                                                       |

The command `cat` print all lines of a file. The command `tac` prints the same in reverse order.

| Command     | Description                                                                                                         |
|-------------|---------------------------------------------------------------------------------------------------------------------|
| `cat -A`    | Displays non-printing characters, showing tabs (`^I`), ends of lines (`$`), and non-printing characters as `^` and M- notation. |
| `cat -b`    | Numbers all non-blank output lines, starting at 1.                                                                  |
| `cat -n`    | Numbers all output lines, starting at 1.                                                                            |
| `cat -s`    | Squeezes multiple adjacent blank lines into a single blank line.                                                    |
| `tac`       | Concatenates and prints files in reverse, line by line.                                                             |

## Using grep

The command `grep` is used to search text strings or regular expressions in files, or using a pipe in command output. This is one of the most important tools in linux. Some examples are:

| Command                | Description                                                                                                         |
|------------------------|---------------------------------------------------------------------------------------------------------------------|
| `grep linda *`         | Searches for the term "linda" in all files in the current directory and displays matching lines.                    |
| `ps aux \| grep http`  | Lists all running processes (`ps aux`) and filters (`grep`) for those containing "http" in their information.       |

Some `grep` options are:

| Option | Description                                                                                             |
|--------|---------------------------------------------------------------------------------------------------------|
| `-i`   | Performs case-insensitive matching, so that searches are not case-sensitive.                            |
| `-v`   | Inverts the match, displaying lines that do not contain the specified pattern.                          |
| `-l`   | Lists only the names of files with at least one match, rather than displaying the matching lines.        |
| `-A5`  | Displays 5 lines after each matching line found in a file (`-A` stands for "after").                     |
| `-B5`  | Displays 5 lines before each matching line found in a file (`-B` stands for "before").                   |
| `-C5`  | Displays 5 lines before and after each matching line found in a file (`-C` stands for "context").         |
| `-R`   | Recursively searches subdirectories, useful for searching through directories and their contents.        |
