## Basic Linux permissions

There are three basic permissions in Linux:

 * **Read** (octal bit 4): You can read files and list files in directories
 * **Write** (octal bit 2): You can modify files and for directories, you can add or delete them.
 * **Execute** (octal bit 1): You can run a file as a command and `cd` within a directory.

To manage permissions you need to have ownership of the file. The user who created the file is the user-owner (`u`), and the primary group of that user becomes the group owner (`g`), and everybody else is in the others owner group (`o`). This is known as `ugo`.

Standard Linux allows one user-owner and one group-owner per file.

| Command                    | Explanation                                                                       |
|----------------------------|-----------------------------------------------------------------------------------|
| `chown`                    | Changes the ownership of a file or directory.                                      |
| `chown anna myfile`        | Changes the owner of `myfile` to user `anna`.                                      |
| `chown anna:sales /data/sales` | Changes the owner of `/data/sales` to user `anna` and the group to `sales`.   |
| `chgrp`                    | Changes the group ownership of a file or directory.                                |
| `chgrp sales /data/sales`  | Changes the group of `/data/sales` to `sales`.                                     |
| `chmod`                    | Changes the file mode bits (permissions) of a file or directory.                   |
| `chmod 750 myfile`         | Sets the permissions of `myfile` to `rwxr-x---` (owner can read, write, execute; group can read, execute; others have no permissions). |
| `chmod +x myscript`        | Adds execute permissions to `myscript` for all users.                              |

## Advance Linux permissions

Set User ID (SUID) (4)
 * On files, it runs the file as the user-owner of that file
Set Group ID (SGID) (2)
 * On files, it runs the file as the group-owner of that file
 * On directories, it sets ownership on newly created items as the group owner of that directory
Sticky Bit (1)
 * Allow user to delete files if the user is file owner or the user is directory owner.

| Command                     | Explanation                                                                                                    |
|-----------------------------|----------------------------------------------------------------------------------------------------------------|
| `chmod u+s myscript`        | Sets the setuid bit on `myscript`, allowing users to run the script with the file owner's permissions.          |
| `chmod u-s myscript`        | Removes the setuid bit from `myscript`.                                                                         |
| `sudo find / -perm /4000`   | Finds all files with the setuid bit set, starting from the root directory.                                      |
| `diff filea fileb`          | Compares `filea` and `fileb` line by line, showing the differences between them.                                |
| `chmod g+s /data`           | Sets the setgid bit on the `/data` directory, causing files created within to inherit the group ID.             |
| `chmod g-s /data`           | Removes the setgid bit from the `/data` directory.                                                              |
| `chmod +t /data`            | Sets the sticky bit on the `/data` directory, ensuring that only the file's owner can delete or rename it.      |
| `chmod -t /data`            | Removes the sticky bit from the `/data` directory.                                                              |

## Managing umask

The shell setting `umask` defines a mask that will be subtracted from the default permissions. The default permissions on directories are `777` and on files would be `666`. The script `/etc/bashrc` set some settings of the default permissions for all users or `/etc/skel/.bashrc` for all new users. It can be set as well in individual user's `.bashrc` files.

| Command     | Explanation                                                                                       |
|-------------|---------------------------------------------------------------------------------------------------|
| `umask`     | Displays the current user file-creation mode mask, which determines default file and directory permissions. |
| `umask 022` | Sets the user file-creation mode mask to `022`, resulting in default permissions of `755` for directories and `644` for files. |
| `umask 027` | Sets the user file-creation mode mask to `027`, resulting in default permissions of `750` for directories and `640` for files. |
