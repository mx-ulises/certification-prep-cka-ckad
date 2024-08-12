## Using tar

The default backup utility is `tar` (Tape Archiver) and it is the foundation of all og them. It is commonly used in a generic way to create and distribute software. You can add compression to archives by using the right flag.

| Command                                                     | Explanation                                                                                     |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| `tar cvf backupfile.tar /etc /home`                         | Creates an uncompressed `tar` archive named `backupfile.tar` of the `/etc` and `/home` directories. |
| `tar czvf backupfile.tar.gz /etc /home`                     | Creates a `gzip` compressed `tar` archive named `backupfile.tar.gz` of the `/etc` and `/home` directories. |
| `tar cjvf backupfile.tar.bz /etc /home`                     | Creates a `bzip2` compressed `tar` archive named `backupfile.tar.bz` of the `/etc` and `/home` directories. |
| `tar cJvf backupfile.tar.xz /etc /home`                     | Creates an `xz` compressed `tar` archive named `backupfile.tar.xz` of the `/etc` and `/home` directories. |
| `tar tvf backupfile.tar`                                    | Lists the contents of the `tar` archive named `backupfile.tar` without extracting it.           |
| `tar xvf backupfile.tar -C /destination`                    | Extracts the contents of `backupfile.tar` to the specified `/destination` directory.            |

By default backup are not compressed. You need to connect multiple specialized tools instead of building in the required functionality to all tools. The compression utilities that can be used are:

| Command                               | Explanation                                                                                   |
|---------------------------------------|-----------------------------------------------------------------------------------------------|
| `gzip`                                | Compresses files using the `gzip` algorithm.                                                  |
| `gunzip backupfile.tar.gz`            | Decompresses a `gzip` compressed file (`backupfile.tar.gz`).                                  |
| `bzip2`                               | Compresses files using the `bzip2` algorithm.                                                 |
| `bunzip2 backupfile.tar.bz`           | Decompresses a `bzip2` compressed file (`backupfile.tar.bz`).                                 |
| `xz`                                  | Compresses files using the `xz` algorithm.                                                    |
| `xz -d backupfile.tar.xz`             | Decompresses an `xz` compressed file (`backupfile.tar.xz`).                                   |

## Using rsync

The rsync utility helps to synchronize files between different directories. You can use it locally, but also remotely if sshd is running on the remote host. It is a great utility to ensure the contents of a directory is synchronized with the contents of a remote directory.

| Command                                                                       | Explanation                                                                                                   |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| `rsync -azP <local_dir> <user>@<remote_server>:<remote_dir>`                  | Synchronizes `<local_dir>` to `<remote_dir>` on `<remote_server>` using compression (`-z`) and progress (`-P`). |
| `rsync -azP --delete <local_dir> <user>@<remote_server>:<remote_dir>`         | Synchronizes `<local_dir>` to `<remote_dir>` on `<remote_server>` with compression, progress, and deletes files in the remote directory that are not present in the local directory. |

## Using dd

The dd utility was developed to work with complete devices. It makes possible to make an ISO of an optical disk, or to clone a complete hard disk. It should not be used for copying files or making archives of files.

| Command                                                        | Explanation                                                                                                        |
|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| `dd if=<input_device> of=<device> bs=512 count=1`              | Copies the first 512 bytes from `<input_device>` to `<device>`.                                                     |
| `dd if=/dev/zero of=<device> bs=512 count=1`                   | Writes 512 bytes of zeroes to `<device>`, effectively clearing the first 512 bytes of the device.                  |
| `xxd <device>`                                                 | Displays a hexadecimal dump of `<device>`.                                                                         |
| `dd if=<device> of=<iso_file> bs=1M`                           | Creates an ISO image file from `<device>` by copying its contents in 1MB blocks to `<iso_file>`.                   |
