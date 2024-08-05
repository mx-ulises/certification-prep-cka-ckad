## File access control lists

Access Control Lists (ACL) allow administrators to grant permissions to more than one user and/or more than one group.  They are supported by all modern filesystems as a default. It can be used in differen scenarios such as in a shared user environment, one user have full access and other users have read-only access, or in a dev environment where a developer may require access to aserver document root. A regular ACL will take care of all currently existing files, and a default ACL will take care of all new files.

| Command                                       | Explanation                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `getfacl`                                     | Gets the file access control list (ACL) for a specified file or directory.                        |
| `setfacl`                                     | Sets the file access control list (ACL) for a specified file or directory.                        |
| `groupadd account`                            | Adds a new group named `account`.                                                                 |
| `mkdir -p /data/sales`                        | Creates the directory `/data/sales` and any necessary parent directories.                         |
| `chgrp sales /data/sales`                     | Changes the group ownership of `/data/sales` to `sales`.                                          |
| `chmod 770 /data/sales`                       | Sets the permissions of `/data/sales` to read, write, and execute for owner and group, no access for others. |
| `setfacl -R -m g:account:rX /data/sales`      | Recursively sets an ACL, giving the `account` group read and execute permissions on `/data/sales`.|
| `setfacl -m d:g:account:rx /data/sales`       | Sets a default ACL for new files in `/data/sales`, giving the `account` group read and execute permissions. |
| `setfacl -x g:account /data/sales`            | Removes the ACL entry for the `account` group from `/data/sales`.                                 |
| `getfacl /data/sales/file1`                   | Gets the file access control list (ACL) for `/data/sales/file1`.                                  |

## Filesystem attributes

Posix defines a number of attributes that can be used to add security to files.

| Command                 | Explanation                                                                                       |
|-------------------------|---------------------------------------------------------------------------------------------------|
| `chattr`                | Changes file attributes on a Linux file system.                                                   |
| `lsattr`                | Lists the attributes of files on a Linux file system.                                             |
| `chattr +i <file>`      | Sets the 'immutable' attribute on the specified file, preventing it from being modified.          |
| `lsattr <file>`         | Lists the attributes of the specified file.                                                       |
| `chattr -i <file>`      | Removes the 'immutable' attribute from the specified file, allowing it to be modified.            |
| `man chattr`            | Displays the manual page for `chattr`, providing detailed information about the command and its options. |

## Pluggable authentication modules

Plugable Authentication Modules (PAM) helps to separate the specific authentication approach from a binary that needs it. PAM also provide modules that may be used by different binaries to enhance security. Binaries using PAM, the configuration is stored in `/etc/pam.d/`. Pam libraries are in `/lib64/security/`

| Command                          | Explanation                                                                                       |
|----------------------------------|---------------------------------------------------------------------------------------------------|
| `pam_tally2`                     | Displays and manages the count of login attempts on a system, used for tracking login failures.   |
| `ldd $(which login)`             | Displays the shared libraries required by the `login` command.                                    |
| `man pam_securetty`              | Displays the manual page for the `pam_securetty` module, which restricts root login to secure terminals. |
| `loginctl list-sessions`         | Lists the current user sessions on the system.                                                    |
| `loginctl terminate-session 6`   | Terminates the specified user session (session ID 6).                                             |
| `vim /etc/pam.d/system-auth`     | Opens the PAM system authentication configuration file for editing using `vim`.                   |
