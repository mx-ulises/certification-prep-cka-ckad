## Boot procedure

The computer start either with BIOS or UEFI, and them will load the grub2 bootloader that will load the kernel that will load the drivers of initramfs and finally systemd. Systemd will boot several system and finally load the shell.

## Systemd targets

A systemd target is a group of units, and if it has the isolate property set, it defines the state a system starts in:
 * `emergency.target` used for minimal troubleshooting
 * `rescue.target` used for troubleshooting with more complete system
 * `multi-user.target` used to start full system minus graphical interface
 * `graphical.target` full system with graphical interface

GRUB menu can set these targets or you can set the default with systemd.

s| Command                                      | Explanation                                                                                       |
|----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl list-unit-files -t target`        | Lists all unit files of type `target` available on the system.                                    |
| `systemctl cat <target>`                     | Displays the content of the specified target unit file.                                           |
| `systemctl get-default`                      | Shows the default target (runlevel) that the system will boot into.                               |
| `systemctl set-default <target>`             | Sets the specified target as the default target for the system to boot into.                      |
| `systemctl start <target>`                   | Starts the specified target, bringing the system into the state defined by that target.           |
| `systemctl isolate <target>`                 | Switches to the specified target immediately, stopping all other units not required by it.        |
| `systemctl reboot`                           | Reboots the system.                                                                               |

## GRUB parameters and configuration

Specific boot options can be passed while booting in boot menu. You can enter boot options on the line that starts with linux. You can modify the target it can start by adding the parameter `systemd.unit=<target>`.

Grub configurations are in `grub.cfg`. To make modifications, they are written in `/etc/default/grub.cfg`.

| Command                                        | Explanation                                                                                       |
|------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `vim /etc/default/grub.cfg`                    | Opens the GRUB configuration file for editing using `vim`.                                        |
| `grub2-mkconfig -o /boot/grub2/grub.cfg`       | Generates a new GRUB configuration file at the specified location.                                |
| `grub2-mkconfig -o /boot/EFI/efi/boot/grub.cfg`| Generates a new GRUB configuration file for EFI systems at the specified location.                |
| `man 7 bootparam`                              | Opens the manual page for boot parameters, detailing kernel boot parameters and their usage.      |
| `info -f grub -n 'Simple configuration'`              | Opens the GNU info documentation for GRUB, focusing on the "Simple configuration" section.        |

## Init and Upstart

Init has been the default system for starting service Linux, now it is replaced by systemd. The kernel started `/sbin/init`, and init started init scripts in `/etc/init.d`. Configuration was done in `/etc/inittab`. Upstart was an improvement to init, that introduced event-driven starting of service. They have runlevels:
 * 0: halt
 * 1: rescue mode
 * 2: rescue mode with network
 * 3: multi-user mode
 * 4: not used
 * 5: graphical mode
 * 6: reboot

| Command    | Explanation                                                                                       |
|------------|---------------------------------------------------------------------------------------------------|
| `init 0`   | Shuts down and powers off the system by changing the runlevel to 0.                               |
| `telinit 0`| Equivalent to `init 0`, changes the runlevel to 0 to shut down and power off the system.          |

## Troubleshooting

 * If grub is not loading, the best way to troubleshoot is to use Rescue disk.
 * If kernel doesnt load, you need to use grub menu.
 * If kernel load but still have problems, you can add the flag `init=/bin/bash` in grub.
 * If you have problems in shell after loading systemd services, you can use `emergency.target` or `rescue.target` in grub.

| Command                         | Explanation                                                                                       |
|---------------------------------|---------------------------------------------------------------------------------------------------|
| `exec /usr/lib/systemd/systemd` | Replaces the current shell process with the `systemd` process, effectively reinitializing the system.|
| `mount -o remount,rw /`         | Remounts the root filesystem (`/`) with read-write permissions.                                   |

## Rescue disk

| Command                           | Explanation                                                                                       |
|-----------------------------------|---------------------------------------------------------------------------------------------------|
| `lsblk`                           | Lists information about all available or the specified block devices.                             |
| `lvs`                             | Displays information about logical volumes.                                                       |
| `vgs`                             | Displays information about volume groups.                                                         |
| `lvdisplay`                      | Displays detailed information about logical volumes.                                              |
| `vgchange -a y`                   | Activates all logical volumes in the volume group.                                                |
| `mount /dev/cs/root /mnt`         | Mounts the logical volume `/dev/cs/root` to `/mnt`.                                               |
| `chroot .`                        | Changes the root directory to the current directory (`.`).                                         |
| `mount -a`                        | Mounts all filesystems defined in `/etc/fstab`.                                                   |
| `mount -o bind /dev /mnt/dev`     | Binds the `/dev` directory to `/mnt/dev`, making device files accessible in the chroot environment.|
| `mount proc /proc -t proc`        | Mounts the `proc` filesystem at `/proc`.                                                          |
| `grub2-install /dev/sda`          | Installs GRUB2 bootloader on the specified device (`/dev/sda`).                                   |
| `chroot /mnt/sysroot`             | Changes the root directory to `/mnt/sysroot` for the current session.                             |
