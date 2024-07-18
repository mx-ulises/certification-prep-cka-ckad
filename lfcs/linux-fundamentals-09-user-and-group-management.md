## Users and role of ownership

A user is an entity that is used to grant permissions to specific parts of Linux. Users are used by services on Linux, running services with a restricted user account avoids running them with too many permissions as root user. There are also IT users for the main IT staff of the company.

Ownership tell you which user own a file on Linux. If a user creates a file, that user will be the owner. Every Linux user is part of at least one group. Many Linux distributions uses private groups, which means that while creating a user, a group with will be created which has same name as the user and the only member and owner is the created user.

| Command  | Description                                                                                     |
|----------|-------------------------------------------------------------------------------------------------|
| `id`     | Displays user identity, including user ID (UID), group ID (GID), and group memberships.         |
| `ps aux` | Shows a detailed list of all running processes, including user, process ID, CPU and memory usage, and command. |
| `ls -l`  | Lists directory contents in long format, showing file permissions, number of links, owner, group, size, and modification date. |


## Create Users

To create an user, `useradd` is the main command to do that. There are some alternatives. When create user, default settings from `/etc/login.defs` are applied.

| Command       | Description                                                                                     |
|---------------|-------------------------------------------------------------------------------------------------|
| `useradd`     | Creates a new user account without creating a home directory.                                   |
| `useradd -m`  | Creates a new user account and a home directory for the user.                                   |
| `useradd -S`  | Creates a new system account, typically with no password and no home directory.                 |
| `useradd -D`  | Displays or changes the default values for new user accounts.                       |
| `userdel`     | Deletes a user account from the system.                                                         |
| `userdel -rf` | Deletes a user account along with their home directory and mail spool.                          |

## Create Groups

The command `groupadd` help to create groups and `usermod` to add users to a group as a secondary group. Add members to a secondary group won't change ownership of created files by the user.

| Command                  | Description                                                                                      |
|--------------------------|--------------------------------------------------------------------------------------------------|
| `groupadd`               | Creates a new group on the system.                                                                |
| `usermod`                | Modifies a user account, including changes to user properties and group memberships.             |
| `usermod -aG`            | Adds a user to supplementary groups without removing current group memberships.                   |
| `grep user /etc/group`   | Searches for occurrences of 'user' in the `/etc/group` file, displaying group membership details. |
| `groupdel`               | Deletes a group from the system.                                      |
| `groupmod`               | Modifies a group, including changes to group properties and membership.                   |

## Managing User and Group properties

Users and their properties are stored in `/etc/passwd`. Common properties are:
 * `UID`: The unique user ID
 * `GID`: The ID of the primary group of the user
 * `GECOS`: Also referred as the comment field. This is an optional description field for the user
 * Home directory: The default directory where the user will land after loggin in
 * Shell: The program that is started after user login (usually `/bin/bash`, for no shell it is used `/sbin/nologin`)
 * Passwords are stored in `/etc/shadow` and not in `/etc/passwd`.

Groups are stored in `/etc/group`, this includes users that are a group member as a secondary group. Primary group membership is uniquely administrated through `/etc/passwd`. Groups doesn't have significant other properties.

| Command         | Description                                                                                          |
|-----------------|------------------------------------------------------------------------------------------------------|
| `getent passwd` | Retrieves and displays entries from the passwd database, showing user account information.           |
| `vipw`          | Safely edits the passwd file using the default text editor, locking the file to prevent simultaneous editing. |
| `vigr`          | Safely edits the group file using the default text editor, locking the file to prevent simultaneous editing. |

## Configuring defaults for new users

Users are added with `useradd -D` and the default configuration is stored in `/etc/login.defs`. Most important settings are the password settings, encryption methods, enable user groups and default directory. It wont apply to old users. Content of `/etc/skell` is copied to user home directory upon user creation, this is a directory that helps to distribute files to new user's home directories. Linux doesn't offer an easy solution to apply new defaults to existing users.

## Managing passwords

User can change their passwords using `passwd`. Administrator can manage passwords with some commands. Password information is stored in `/etc/shadow`

| Command                                | Description                                                                                   |
|----------------------------------------|-----------------------------------------------------------------------------------------------|
| `passwd`                               | Changes a user's password.                                                                    |
| `chage`                                | Changes user password expiry information.                                                     |
| `chage -l`                             | Lists password expiry information for a user.                                                 |
| `passwd -S`                            | Displays the status of a user's password.                                                     |
| `chpasswd`                             | Reads a file of username:password pairs and updates the passwords in batch mode.              |
| `echo password \| passwd --stdin username`      | Sets the password for the specified user by reading from standard input.                      |
| `echo "username:password" \| chpasswd` | Sets the password for the specified user using a username:password pair input.                |
| `man 5 shadow`                         | Displays the manual page for the shadow file format, which stores password and account information securely. |

## Managing current sessions

Session management is to understand who is currently login and how to manage current session such as terminate or get details about the session. Useful when unahutorized access is granted and need to kick out the attacker.

| Command                        | Description                                                                                       |
|--------------------------------|---------------------------------------------------------------------------------------------------|
| `who`                          | Displays information about currently logged-in users.                                              |
| `w`                            | Displays information about currently logged-in users, including their activity and processes.      |
| `loginctl`                     | Controls the systemd login manager service and displays session and user information.              |
| `loginctl list-sessions`       | Lists active login sessions with session IDs and user details.                                     |
| `loginctl show-session`        | Shows detailed information about a specific login session.                                         |
| `loginctl show-user`           | Shows detailed information about a specific user, including sessions and properties.               |
| `loginctl terminate-session`   | Terminates (logs out) a specific login session.                                                    |
