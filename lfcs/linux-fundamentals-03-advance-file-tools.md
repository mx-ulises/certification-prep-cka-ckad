## Links

A Link is a filesystem entry that refers to another file or directory. There are:

 * Hard links: Pointing to the same inode on the same filesystem. An inode is an administration element that keep details of the file within the filesystem, such as in which block of the disk is written, etc... The hardlink is a name that refer to an inode. They allow to modify multiple filenames refering to the same block only once.
 * Symbolic Links: They are shorcuts and add additional flexibility. Symbolic links can exist on a directory and cross-device symbolic links can be used. The symbolic links depend on the hard link they are created for, so if the file is deleted or moved, the symbolic link is going to be broken.

Commands:

| Command | Description |
|---------|-------------|
| `ls -il` | Lists files with inode numbers and detailed info (permissions, owner, size, etc.). |
| `ln` | Creates a hard link, making another name for a file with the same inode. |
| `ln -s` | Creates a symbolic (soft) link, which is a pointer to another file or directory. |


## Find files

| Command | Description |
|---------|-------------|
| `find / -name "host"` | Searches for files named exactly "host" in the root directory (`/`) and its subdirectories. |
| `find / -name "host*"` | Searches for files with names starting with "host" in the root directory (`/`) and its subdirectories. |
| `find / -user "linda"` | Searches for files owned by the user "linda" in the root directory (`/`) and its subdirectories. |
| `find / -size +2G` | Searches for files larger than 2 gigabytes in the root directory (`/`) and its subdirectories. |
| `find / -user "linda" -exec cp {} /root/linda \;` | Searches for files owned by "linda" and copies them to `/root/linda`. The `{}` represents each found file, and the `\;` indicates the end of the `-exec` command. |
| `find / -perm /4000` | Searches for files with the SUID (Set User ID) permission in the root directory (`/`) and its subdirectories. |
| `find / -type f` | Searches for regular files (not directories or special files) in the root directory (`/`) and its subdirectories. |
| `find /etc -name "*conf" -printf '%s, %p\n'` | Searches for files in `/etc` ending with "conf" and prints their size and path. |
| `xargs` | Executes a command using input from standard input as arguments. Useful with commands like `find` or `grep`. |
| `sort` | Sorts lines of text or files in ascending order. |
| `sort -rn` | Sorts lines of text or files in descending numerical order. |
| `locate` | Quickly finds files by name using a pre-built database. |
| `updatedb` | Updates the database used by `locate` with the current file system contents. |
| `which` | Shows the full path of a command, indicating where it is located in the system's PATH. |

The command `find` is powerful to find any file no matter where it is stored, but it is also slow. Other options as `locate` are much faster, but it works on a database that needs to be defined using updatedb and being updated periodically. The command `which` is useful to find the exact location of binary files from the `$PATH` variable.

## File compression

The command `tar` is the Tape ARchiver, it is used to compress, extract or list. thi s command uses BSD Unix stule and System-V Unix style ways to provide argument (with `-` or without it).

To compress we can use `gzip`, `bzip2`, `xzip`, `zip`. The differences among them are in regards on compression utilities are how much time they take and how much they compress.

| Command | Description |
|---------|-------------|
| `tar -cvf my_archive.tar /home` | Creates a tar archive named `my_archive.tar` containing the contents of the `/home` directory. |
| `tar -xvf my_archive.tar` | Extracts the contents of `my_archive.tar` into the current directory. |
| `tar -xvf my_archive.tar -C /home` | Extracts the contents of `my_archive.tar` into the `/home` directory. |
| `tar -tvf my_archive.tar` | Lists the contents of `my_archive.tar` without extracting them. |
| `file my_archive.tar` | Determines and displays the type of `my_archive.tar`, typically showing it's a tar archive. |
| `tar -czvf my_archive.tar.gz /home` | Creates a compressed tar archive named `my_archive.tar.gz` using gzip compression, containing the contents of the `/home` directory. |
| `tar -cjvf my_archive.tar.bz2 /home` | Creates a compressed tar archive named `my_archive.tar.bz2` using bzip2 compression, containing the contents of the `/home` directory. |
| `tar -cJvf my_archive.tar.xz /home` | Creates a compressed tar archive named `my_archive.tar.xz` using xz compression, containing the contents of the `/home` directory. |
| `gzip my_archive.tar` | Compresses `my_archive.tar` using gzip, creating `my_archive.tar.gz`. |
| `gunzip my_archive.tar.gz` | Decompresses `my_archive.tar.gz`, restoring the original `my_archive.tar`. |

## Mount a Filesystem

In root directories we have several directories and they are independent of the directories. To use a new device we need to mount it to a directory such as `/dev/sda1`, other devices can be mounted in the `/mnt` directory (for example `/dev/sdb1`)

| Command | Description |
|---------|-------------|
| `lsblk` | Lists information about all available block devices (disks, partitions, etc.) in a tree-like format, showing their size, type, and mount points. |
| `mount` | Displays all currently mounted filesystems. When used with additional options, it mounts a filesystem to a specified directory. |
| `mount /dev/sdb /mnt` | Mounts the filesystem on the device `/dev/sdb` to the directory `/mnt`. |
| `umount /dev/sdb` | Unmounts the filesystem on the device `/dev/sdb`, making it safe to remove or modify the device. |
| `df -h` | Displays disk space usage for all mounted filesystems in a human-readable format (sizes are shown in KB, MB, GB, etc.). |
| `findmnt` | Displays a tree of all mounted filesystems or searches for a specific filesystem based on specified criteria. |
