## Storage solutions in Linux

To address disks, Linux provides devices files in `/dev`
 * `/dev/sda`: First SCSI hard disk
 * `/dev/sdb`: Seccond SCSI hard disk
 * `/dev/vda`: KVM hard disk
 * `/dev/nvme0n1`: First NVME hard disk
 * `/dev/sr`: Optical drive

## Create MBR and GPT partitions

There are to systems to create partitions:
 * Master Boot Record (MBR): Introduced in 1981 for PC standard. You can have up to 4 partitions to the 512 bytes boot sector. To configure more than 4 partitions, 3 are configures as primary and one is configured as extended partition. Extended partitions can only be used to include logical partitions and the first local partition is alwats numered as partition 5. You use this when you want to add a new disk.
 * Grid Partition Table (GPT): Introduced in 2010. It can allow up to 128 partitions. GPT is required on disks bigger than 2TB (that is the limit for MBR).

| Command            | Explanation                                                                                       |
|--------------------|---------------------------------------------------------------------------------------------------|
| `lsblk`            | Lists information about all available or the specified block devices, including their partitions and mount points. |
| `fdisk /dev/sdb`   | Opens the `fdisk` utility for managing partitions on the disk `/dev/sdb`.                         |
| `gdisk /dev/sdc`   | Opens the `gdisk` utility for managing partitions on the disk `/dev/sdc` using the GPT partitioning scheme. |

## Create and mounting file systems

To use a partition, you need to create a file system on top of it. The file systems are used to store files:
 * `Swapfs` is a swap file system.
 * `Initramfs` is a file system that is written to the Init RAM FS, which is used while booting.
 * `XFS` and `Ext4` are generic file systems.

| Command                  | Explanation                                                                                  |
|--------------------------|----------------------------------------------------------------------------------------------|
| `mkfs.xfs /dev/sdb1`     | Creates an XFS filesystem on the partition `/dev/sdb1`.                                       |
| `mkfs.ext4 /dev/sdc1`    | Creates an ext4 filesystem on the partition `/dev/sdc1`.                                      |


To use a partition that contains a file system, it must be mounted. By mounting a partition, it is connected to a directory. While weiting to a directory that contains a mounted partition, writes are actually commited to the moutned device. When the conected device is unmounted, you wont see the files that are witten anymore. Notice that mounting is also required for devices like DVC or CD-ROM.

The file `/etc/fstab` is a configuration file that contains names of devices and where to mount them at boot time.

| Command                    | Explanation                                                                                   |
|----------------------------|-----------------------------------------------------------------------------------------------|
| `mount`                    | Displays all currently mounted filesystems.                                                   |
| `mount /dev/sdb1 /mnt`     | Mounts the filesystem on the partition `/dev/sdb1` to the directory `/mnt`.                   |
| `mount -a`                 | Mounts all filesystems mentioned in `/etc/fstab`.                                             |
| `umount /mnt`              | Unmounts the filesystem currently mounted at the directory `/mnt`.                            |
| `lsof /mnt`                | Lists open files and processes that are using the filesystem mounted at `/mnt`.               |
| `lsblk`                    | Lists information about all available or specified block devices, including their partitions and mount points. |
| `df -h`                    | Displays disk space usage in a human-readable format.                                         |
| `findmnt`                  | Finds and displays a list of currently mounted filesystems.                                   |
