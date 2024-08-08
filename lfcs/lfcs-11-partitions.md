## Disk storage and partitions

To use disk storage, you should create a partition that should be created to divide a disk into one or more logical areas of the storage. On a partition, the filesystem is created to take care of file allocation. The filesystem is a logical collection of files that comes with solutiosn to allocate the file blocks to disk.

Advanced storage allocation solutions besides partitions are also available:
 * Logical Volume Manager (LVM) defines flexible volumes for storage allocation
 * Stratis is used for t hin provisioning of storage
 * Volume managing filesystems like ZFS and Btrfs combine storage allocation unit with a filesystem.

To create a partition, you need a block device. Common disk block device names includes:
 * `sda`: SCSI/SATA
 * `vda`: virtual disk that uses the kvm virtio driver
 * `xvda`: virtual disk in a Xen environment
 * `nvme0n1`: first NVME disk

On the disk devices, partitions are addressed by adding a number (`sda1`) or a letter p followed by a number (`nvme0n1p1`). Block device names are allocated while booting and for that reason, they are subject to change. To address disk devices in a persistent way, labels or UUIDs can be used.

| Command | Explanation |
|---------|-------------|
| `fdisk` | A command-line utility to create and manage partitions on a hard disk. |
| `parted` | A command-line tool to manage disk partitions, supporting both MBR and GPT partitioning. |
| `gdisk` | A text-mode partitioning tool for GPT disks. |
| `lsblk` | Lists information about all available or the specified block devices. |

## MBR partitions

Master Boot Record (MBR) is part of the original PC standard. It is a 512 byte sector used on diskto store the stage one boot loader, as well as the partition table. MBR reserves 64 bytes for the partition table. In 64 bytes, 4 primary partitions can be created with a maximum size of 2 TiB. To create more than 4 partitions, one primary partition can be assigned as extended partition, in which logical partitions can be created.

| Command                  | Explanation                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| `lsblk`                  | Lists information about all available or the specified block devices.       |
| `fdisk /dev/<device>`    | Opens the `fdisk` utility to create and manage partitions on the specified device. |
| `cat /proc/partitions`   | Displays the partitions recognized by the kernel, including their sizes and major/minor numbers. |

## GPT partitions

GUIF Partition Table (GPT) was introduced in the 2000 to deal with MBR shortcommings. It address up to 128 partitions directly, without making a difference between primary, extended and logical partitions. Maximum disk size is 18 Exabyte. All modern computers and operating system support GPT.

| Command                  | Explanation                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| `gdisk /dev/<device>`    | Opens the `gdisk` utility to create and manage GPT partitions on the specified device. |
