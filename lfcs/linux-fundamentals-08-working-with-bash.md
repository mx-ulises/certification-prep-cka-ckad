## Redirectiong I/O and Pipe

Redirection is used to manipulate input and output of commands.
 * Standard input (0): `<` Is for a file descriptor from your keyboard, it is not used that often.
 * Standard output (1): `>` Is the result sent to the scren
 * Standard error (2) `2>` Redirect standard error to send error output to somewhere else

| Command                                 | Description                                                                                                   |
|-----------------------------------------|---------------------------------------------------------------------------------------------------------------|
| `sort < /etc/services`                  | Sort the contents of the `/etc/services` file and display the sorted output.                                  |
| `ls > ~/myfile`                         | List directory contents and redirect the output to the file `myfile` in the home directory.                   |
| `who >> ~/myfile`                       | Append the output of the `who` command to the file `myfile` in the home directory.                            |
| `grep -R root /proc 2> /dev/null`       | Recursively search for the string "root" in the `/proc` directory, redirecting any error messages to `/dev/null`. |
| `grep -R root /etc &> ~/myfile`         | Recursively search for the string "root" in the `/etc` directory, redirecting both stdout and stderr to `myfile` in the home directory. |

A pipe is used to send the output of one command to be used as input for a second command. The `tee` command combines redirection and piping; it allows you to write output somewhere, and at the same time use it as input for another command.

| Command                                   | Description                                                                                                 |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| `ps aux \| grep http`                     | Display a list of all running processes and filter the output to show only those containing "http".         |
| `ps aux \| tee psfile \| grep ssh`        | Display a list of all running processes, write the output to `psfile`, and filter the output to show only those containing "ssh". |

## Working with history

Commands a user types are written to `~/.bash_history`. This file is persistent, meaning that if you reboot your terminal you will see the commands. The `history` command is used to repead commands from this file. You can use `Ctrl+r` for reverse search of the history file and `!<offset>` to run the command in the given offset.

| Command      | Description                                                                        |
|--------------|------------------------------------------------------------------------------------|
| `history`    | Displays the command history list with line numbers.                               |
| `history -c` | Clears the current command history.                                                |
| `history -w` | Writes the current command history to the history file.                            |
| `history -d` | Deletes a specific entry from the command history. Usage: `history -d <offset>`.   |
| `!<offset>`         | Executes the command at position `<offset>` in the history list.                          |
| `ctrl+r`     | Initiates a reverse search through the command history.                            |

## Command completion

The `Tab` key can be used for command line completition and it works on different items for automate completion:
 * Commands
 * Variables
 * File names

Sometimes you need to press `Tab` more than once to find the right option if there are many alternatives. You need to install the `bash-completion` package for additional completion features.

## Variables

A variable is a label to which a dynamic value can be assigned. They are convenient for scripting: Define a variable once, and use in a flexible wat in different environments. It helps to decoupling features. System variables contain default settings used by Linux.

Environment variables can be set for application usage:

```
varname=value
echo $varname
```

By default, variables are only known to the current shell. To have them available in other shells, you can use `export` to export it to all subshells. The `env` command provide a list of environment variables.

| Command/Variable | Description                                                                        |
|------------------|------------------------------------------------------------------------------------|
| `env`            | Displays the current environment variables.                                        |
| `export`         | Sets or exports an environment variable. Usage: `export VAR=value`.                |
| `$PATH`          | An environment variable that specifies the directories in which executable programs are located. |

## Other features

The command `alias` allows you to define your own commands. By default, alias are set througth `/etc/profile`.

| Command  | Description                                                           |
|----------|-----------------------------------------------------------------------|
| `alias`  | Creates a shortcut for a command or a series of commands. Usage: `alias name='command'`. |
| `unalias`| Removes an alias. Usage: `unalias name`.                              |

Bash also includes convenient keyboard shortcuts

| Shortcut | Description                                            |
|----------|--------------------------------------------------------|
| `Ctrl+l` | Clears the terminal screen.                            |
| `Ctrl+u` | Deletes everything from the cursor to the beginning of the line. |
| `Ctrl+a` | Moves the cursor to the beginning of the line.         |
| `Ctrl+e` | Moves the cursor to the end of the line.               |
| `Ctrl+c` | Cancels the current command or process.                |
| `Ctrl+d` | Logs out of the current session or closes the terminal if pressed on an empty line. |

## Bash Startup Files

The file `/etc/environment` contains a list of variables and is the first file that is processed while starting bash (empty by default on Red Hat)

The file `/etc/profile/` is excuted while users login and should be not modified directly, instead use the following alternatives:
 * `/etc/profile.d` is used as a drop-in directory that contains additional configurations
 * `~/.bash_profile` can be used as a user-specific version
 * `~/.bash_logout` is proccesed when a user logs out

The file `/etc/bashrc` is proccesed every time a subshell is started.
 * `~/.bashrc` is for user-specific configurations

The files `/etc/profile` and `/etc/bashrc` are similar but the `/etc/profile` file should be used when first login happens, for `/etc/bashrc` is when you already know the user is in, so we can do reduced configurations.

These files are shell scripts and should be modified as such. After changing the scripts, they should be reloaded either by login again or running `source <filename>` command for the configuration file that was changed.
