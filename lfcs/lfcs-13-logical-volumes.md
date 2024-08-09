## Logical volume manager (LVM)

The logical volume manager (LVM) adds flexivility to the storage layer, it adds many features that doesn't eist in partitions: Resize, snapshots, software RAID, thin provisioning and multi-device. On top of a logical volume, any filesystem can be created.

LVM has three layers:
 * Physical volume (pvs) represent the block devices
 * Volume group (vg) represents all of the available storage
 * Logical volumes (lv) are created as logical block devices from the volume group and formatted with any filesystem

## Create logical volumes

It is recommended to create a partition and flag it as the LVM partition type. The logical volumes will show as `/dev/vgname/lvname` and `/dev/mapper/vgname-lvname`.

| Command                                      | Explanation                                                                                          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------|
| `gdisk /dev/<device>`                        | Opens the GPT disk partitioning tool to manage partitions on the specified device.                   |
| `lsblk`                                      | Lists information about all available or the specified block devices, showing their mount points.     |
| `pvcreate /dev/<device>`                     | Initializes a physical volume (PV) on the specified device for use in a volume group (VG).            |
| `pvs`                                        | Displays information about the physical volumes (PVs) on the system.                                  |
| `vgcreate vgdata /dev/<device>`              | Creates a volume group (VG) named `vgdata` using the specified physical device.                       |
| `vgs`                                        | Displays information about volume groups (VGs) on the system.                                         |
| `lvcreate -n lvdata -L 2G vgdata`            | Creates a logical volume (LV) named `lvdata` of size 2GB within the `vgdata` volume group.            |
| `lvcreate -n lvdata -l 511 vgdata`           | Creates a logical volume (LV) named `lvdata` using 511 extents from the `vgdata` volume group.        |
| `mkfs.ext4 /dev/vgdata/lvdata`               | Formats the logical volume `/dev/vgdata/lvdata` with the ext4 filesystem.                             |
| `vim /etc/fstab`                             | Edits the `/etc/fstab` file, which contains static information about filesystems to be automatically mounted. |

## Resizing logical volumes

To grow a logical volumen, disk space must be available in the volume group. All Linux filesystems support growing. To shrink a logical volume, you need a filesystem that supports it, XFS doesnt do.

| Command                                               | Explanation                                                                                             |
|-------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| `lvs`                                                 | Lists all logical volumes (LVs) on the system, showing their attributes, sizes, and more.               |
| `vgs`                                                 | Displays information about volume groups (VGs) on the system.                                            |
| `vgextend vgdata /dev/<device>`                       | Adds a physical volume (PV) to an existing volume group (VG) named `vgdata`, allowing it to use more space. |
| `lvextend -r -l +50%FREE /dev/vgdata/lvdata`          | Extends the logical volume `lvdata` by 50% of the free space in the volume group and resizes the filesystem to match. |
| `umount <directory>`                                  | Unmounts the filesystem mounted on the specified directory, making it safe to resize or check.           |
| `e2fsck -f /dev/vgdata/lvdata`                        | Forces a filesystem check on the specified logical volume to ensure its integrity before resizing.       |
| `resize2fs /dev/vgdata/lvdata 2048M`                  | Resizes the filesystem on the logical volume `lvdata` to 2048MB.                                         |
| `lvresize /dev/vgdata/lvdata -L 2048M`                | Resizes the logical volume `lvdata` to 2048MB, without affecting the filesystem.                         |
| `vgresize`                                            | Resizes a volume group (VG) to reflect changes in the size of its underlying physical volumes (PVs).     |
