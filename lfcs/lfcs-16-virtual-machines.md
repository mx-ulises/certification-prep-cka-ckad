## KVM

KVM is the kernel virtual machine. In virtualization, the operating system is installed on top of virtualized hardware. This allows for usage of physical hardware in a more efficient way. A Hypervisor is a software that creates and runs virtual machines. The Linux kernel provide KVM hypervisor. Virtual machines is not containers, they have their own operating system and can be used side by side.

In KVM, you need to check if your CPU supports it. To use nested virtualization, you need hardware support as well as hypervisor level support. A virtual machine needs to be of the same platform as the host, you cannot virtualize ARM on Intel or viceversa, in such case you need emulation instead.

To communicate with KVM, libvirtd are the modules in charge of that, and they are used by virt-manager and other tools.

| Command                                               | Explanation                                                                                      |
|-------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| `grep -e vmx -e svm /proc/cpuinfo`                    | Checks if the CPU supports virtualization by looking for `vmx` (Intel) or `svm` (AMD) flags.     |
| `dnf groups install "Virtualization Host"`            | Installs the necessary packages for setting up a Virtualization Host.                            |
| `dnf install virt-manager`                            | Installs `virt-manager`, a graphical interface for managing virtual machines.                    |
| `systemctl enable --now libvirtd`                     | Enables and starts the `libvirtd` service, which manages virtual machines.                       |
| `virt-manager &`                                      | Launches `virt-manager` in the background.                                                       |

The cloud-init utility is an infrastructure as code solution, which allows a user to easily deploy virtual machines on supported platforms, including:
 * KVM
 * OpenStack
 * AWS
 * Azure
 * Etc

To use it, you need to create a YAML file with installation instructions. The YAML file is consumed by `virt-install` that fully automates the VM creation in KVM.

| Command                                                                                                                                       | Explanation                                                                                                          |
|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| `virt-install --name fedora01 --memory 1024 --vcpus 2 --graphics none --os-variant detect=on --import --disk <image>,size=4 --control pty,target_type=virtio --serial pty --cloud-init user-data=user-data.yaml` | Installs a virtual machine named `fedora01` with 1024MB memory, 2 vCPUs, no graphics, and other specified parameters, using `cloud-init` for configuration. |
