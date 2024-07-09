## User Priviledges

The `root` user is an unrestricted user, and this should be avoided as much as posible for security reasons.

Instead, you can use `sudo` user, member of a `sudo` group (or `wheel` in Red Hat) that has administrative priviledges.

You can use `su -` to change your loging to an administrative user.

The `sudo` command allow users to run tasks with escalated priviledges. An example of this is `sudo ls /root`. You need to have an account that has administrative priviledges.

##  Command Line

 * Linux is case sensitive. Most commands are lowercase.
 * Commands are often used as options:
    * Long options starts with `--` (`uname --help`)
    * Short options starts with `-` and cand be combined (`ls -ltr`)
 * Commands usually have the option `--help` to show how to use the command.

## Essential Linux commands:

| **Command** | **Description**                                                                                                            |
|-------------|----------------------------------------------------------------------------------------------------------------------------|
| `whoami`    | Displays the username of the current user.                                                                                 |
| `ls`        | Lists the contents of a directory. By default, it shows the files and directories in the current directory.                |
| `ip a`      | Displays all the IP addresses and network interfaces on the system.                                                        |
| `cat`       | Concatenates and displays the contents of files. Commonly used to view the contents of a file.                             |
| `passwd`    | Changes the password for a user account. When run as root, it can change the password for any user.                       |
| `touch`     | Creates a new, empty file or updates the timestamp of an existing file.                                                    |
| `pwd`       | Prints the current working directory path.                                                                                 |
| `cd`        | Changes the current directory to the specified path. Can be used with absolute or relative paths.                         |
| `history`   | Displays a list of commands previously entered in the terminal. Useful for recalling past commands and re-executing them. |

## Using man

The `man` command is the Systems Programmers Manual. It provides information about linux command usage, as well as configurations and more.

The `man` is organized in sections with name of the command, a synopsis and description and other information. Most importan sections of a `man` page are:

 * Section 1 for end-user commands
 * Section 8 for administratior (`root`) commands
 * Section 5 for configuration files

You can navigate `man` with following commands:
 * `g` to go to the begining of the page
 * `space` to move to the next page
 * `/` to search for a text, to find next ocurrence press `n`
 * `q` for quiting

You can find what command to use with `mandb` by using `man -k`. Use `sudo mandb` if you get "nothing appropiate"

## Using pinfo

Some commands have extra information available for some commands lsited in the `man` page. The command `pinfo` help to provide more navegable information of a command in linux trerminal.

## Other ways to get help
 * `/usr/share/doc` has some help for additional command
 * `tldr` provide examples of some commands
