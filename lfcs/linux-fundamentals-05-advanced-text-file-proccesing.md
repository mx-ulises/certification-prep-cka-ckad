## Using cut and sort

The command `cut` is a command to filter out columns from text files. Usually you need to specify a delimiter with argument `-d`.

| Command                      | Description                                                 |
|------------------------------|-------------------------------------------------------------|
| `cut`                        | Command to remove sections from each line of files.         |
| `cut -f 3 /etc/passwd`       | Extracts the 3rd field of each line from the `/etc/passwd` file using the default tab delimiter. |
| `cut -d : -f 3 /etc/passwd`  | Extracts the 3rd field of each line from the `/etc/passwd` file using `:` as the delimiter.       |

The command `sort` help to sort results from an output, it is usually used with `cut`.

| Command                                      | Description                                                                                 |
|----------------------------------------------|---------------------------------------------------------------------------------------------|
| `sort`                                       | Sorts lines of text files alphabetically.                                                   |
| `sort -n`                                    | Sorts lines of text files numerically.                                                      |
| `cut -d : -f 3 /etc/passwd \| sort`          | Extracts the 3rd field of each line from the `/etc/passwd` file using `:` as the delimiter, then sorts the output alphabetically. |
| `cut -d : -f 3 /etc/passwd \| sort -n`       | Extracts the 3rd field of each line from the `/etc/passwd` file using `:` as the delimiter, then sorts the output numerically.   |


## Regular Expressions

Regular expressions are text patterns that are used by tools like grep and others. You should always put your regex between single quotes. Regular expresions work in `grep`, `vim`, `awk` and `sed`. Basic regular expresions work with most tools. Extended regex dont alwats work. Use `grep -E` if it is an extended regex.

for more details use `man 7 regex`

## Using tr, awk and sed

The command `tr` translate a sets of characters into another set of characters:

| Command                                  | Description                                                                                   |
|------------------------------------------|-----------------------------------------------------------------------------------------------|
| `echo hello \| tr [:lower:] [:upper:]`   | Converts all lowercase letters in the string "hello" to uppercase using character classes.    |
| `echo hello \| tr [a-z] [A-Z]`           | Converts all lowercase letters in the string "hello" to uppercase using explicit ranges.      |
| `echo hello \| tr [a-m] [n-z]`           | Translates characters in the range `a-m` to the corresponding characters in the range `n-z`.  |

The command `awk` perform complex operations on text. It has advances features that make it into scripting languages.

| Command                                           | Description                                                                                       |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `awk '{ print $0 }' /etc/passwd`                  | Prints each line of the `/etc/passwd` file.                                                       |
| `awk 'length($0) > 40' /etc/passwd`               | Prints lines from the `/etc/passwd` file that have more than 40 characters.                       |
| `awk -F : '{ print $4 }' /etc/passwd`             | Prints the 4th field of each line from the `/etc/passwd` file using `:` as the field separator.   |
| `awk -F : '/user/ { print $4 }' /etc/passwd`      | Prints the 4th field of lines containing the pattern "user" from the `/etc/passwd` file, using `:` as the field separator. |

The command `sed` is a stream editor and allow ytou to edit files in non-visual mode:

| Command                                  | Description                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `sed -n 5p /etc/passwd`                  | Prints the 5th line of the `/etc/passwd` file.                                                           |
| `sed -i s/old/new/g ~/myfile`            | Replaces all occurrences of "old" with "new" in the file `~/myfile`, editing the file in place.          |
| `sed -i -e '2d' ~/myfile`                | Deletes the 2nd line of the file `~/myfile`, editing the file in place.                                  |
