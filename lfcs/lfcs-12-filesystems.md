## Filesystems

Filesystems are used to store files on disk. Blocks are addressed by the filesystems to take care of file allocation on disk. Linux filesystems are compatible with the POSIX standatd, which defines properties that can be stored in the filesystem. The properties are stored in the inode, whose have specific blocks that are used for storage are addressed and other features are stored. These features includes user information, permissions, dates and advanced features that may be used.

Most filesystems are used to store files on block devices, but there are also remote filesystems (like NFS) define how files are stored on remote services, or pseudo filesystems (like `/proc`, `/dev` or `/sys`) that are managed by the kernel and used for internal purposes. The Virtual Filesystem (VFS) is used by the Linux kernel as a generic filesystem interface. VFS allows for working with different filesystems by using specific kernel modules.

## Ext4

This is an old mature and reliable filesystem, it is also the most common and the default in Ubuntu.

| Command                        | Explanation                                                                                     |
|--------------------------------|-------------------------------------------------------------------------------------------------|
| `mkfs.ext4 --help`             | Displays the help information and usage options for the `mkfs.ext4` command.                    |
| `mkfs.ext4 /dev/<device>`      | Creates an ext4 filesystem on the specified device.                                             |
| `mount /dev/<device> /mnt`     | Mounts the specified device to the `/mnt` directory.                                            |
| `umount /mnt`                  | Unmounts the device mounted at the `/mnt` directory.                                            |
| `tune2fs`                      | A utility to adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems.                |
| `tune2fs -l /dev/<device>`     | Lists the superblock information for the specified ext2/ext3/ext4 filesystem.                   |

## XFS

Is a 64-bit high performance filesystem, and it is the default on Red Hat. This has different utilities as well.

| Command                         | Explanation                                                                                   |
|---------------------------------|-----------------------------------------------------------------------------------------------|
| `mkfs.xfs --help`               | Displays the help information and usage options for the `mkfs.xfs` command.                   |
| `mkfs.xfs /dev/<device>`        | Creates an XFS filesystem on the specified device.                                             |
| `xfs_info /dev/<device>`        | Displays information about an XFS filesystem.                                                 |
| `xfs_admin --help`              | Displays the help information and usage options for the `xfs_admin` command.                   |
| `mount /dev/<device> /mnt`      | Mounts the specified device to the `/mnt` directory.                                           |
| `umount /mnt`                   | Unmounts the device mounted at the `/mnt` directory.                                           |

## Swap

Swap can be used to emulate RAM on disk. Swap is always slower than real RAM. It can allocate on partions and as swap files. The partition type for a Swap filesystem should be swap as well (82 on MBR or 8200 in GPT)

| Command                                              | Explanation                                                                                       |
|------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `dd if=/dev/zero of=/swapfile bs=1M count=1024`       | Creates a 1GB swap file by writing zeros to a file named `/swapfile`.                              |
| `mkswap /swapfile`                                   | Sets up a Linux swap area on the `/swapfile`.                                                      |
| `chmod 600 /swapfile`                                | Sets the permissions of `/swapfile` to be readable and writable only by the root user.             |
| `free -m`                                            | Displays the amount of free and used memory in the system in megabytes, including swap space.      |
| `swapon /swapfile`                                   | Enables the `/swapfile` for use as swap space.                                                     |
| `fdisk /dev/sdb`                                     | Opens the `fdisk` utility to manage partitions on the `/dev/sdb` disk.                             |
| `mkswap /dev/<device>`                               | Sets up a Linux swap area on the specified device partition.                                       |
| `swapon /dev/<device>`                               | Enables the specified device partition for use as swap space.                                      |
| `swapon -s`                                          | Displays a summary of swap space currently in use.                                                 |

## Persistently mounting filesystems

Mounts with `mount` are by default not persistent. To make them persitent, they need to be loaded in boot time by using `/etc/fstab`. The mount records must include the device to mount, the directory to mount and the filesystem type.

| Command                | Explanation                                                                                             |
|------------------------|---------------------------------------------------------------------------------------------------------|
| `vim /etc/fstab`       | Opens the `/etc/fstab` file in the `vim` text editor for editing. This file contains filesystem mount configurations. |
| `mount -a`             | Mounts all filesystems mentioned in `/etc/fstab` that are not currently mounted.                        |
| `findmnt`              | Displays a list of currently mounted filesystems in a tree format.                                      |
| `findmnt --verify`     | Verifies `/etc/fstab` entries against currently mounted filesystems and reports any discrepancies.      |

## UUID and labels

Block device names are subject to change, and it is recommended using a solution for persistent naming. The most common solution is an Universal Unique ID (UUID) to assign all block devices. Administrators can manually assign labels for persistent naming.

| Command                                        | Explanation                                                                                           |
|------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| `blkid`                                        | Displays or prints block device attributes, such as UUID, filesystem type, and label.                  |
| `blkid \| grep <device> \| awk '{ print $2 }'`   | Filters the output of `blkid` for a specific device and extracts the second field, typically the UUID. |
| `tune2fs -L <label> /dev/<device>`             | Sets or changes the label of an ext2/ext3/ext4 filesystem.                                             |
| `xfs_admin -L myfs /dev/<device>`              | Sets or changes the label of an XFS filesystem to `myfs`.                                              |

## Systemd mounts and automounts

Now systemd uses `/etc/fstab` as an input file from which its mounts units are created. Systemd mounts are created in `/run/systemd/generator`. Mount files can be created manually as well, and it requires an `[Install]` section with `WantedBy=my.target` to specify which target should be mounted and they have to be added to `/etc/systemd/system`.

| Command                                         | Explanation                                                                                  |
|-------------------------------------------------|----------------------------------------------------------------------------------------------|
| `systemctl enable --now mydata.mount`           | Enables and starts the `mydata.mount` unit, ensuring it is mounted now and at boot.          |
| `systemctl status mydata.mount`                 | Displays the status of the `mydata.mount` unit, including whether it is active and any errors.|
| `systemctl list-unit-files -t mount`            | Lists all mount units, showing whether they are enabled, disabled, or masked.                |
| `findmnt`                                       | Displays a tree of all currently mounted filesystems, showing the source, target, and other details.|

Systemd automount units are used as modifier to a mount unit. If using them, it should be enable, not the mount name. The automount should match the mount filename.

| Command                                           | Explanation                                                                                          |
|---------------------------------------------------|------------------------------------------------------------------------------------------------------|
| `systemctl list-unit-files -t automount`          | Lists all automount unit files, showing whether they are enabled, disabled, or masked.                |
| `systemctl disable --now mydata.mount`            | Disables and stops the `mydata.mount` unit, preventing it from being mounted now or at boot.          |
| `systemctl enable mydata.automount`               | Enables the `mydata.automount` unit, setting it to automatically mount `mydata` when accessed.        |
| `systemctl start mydata.automount`                | Starts the `mydata.automount` unit, making the `mydata` filesystem available for automatic mounting.  |
| `mount \| grep mydata`                             | Checks the list of currently mounted filesystems to see if `mydata` is mounted.                       |
