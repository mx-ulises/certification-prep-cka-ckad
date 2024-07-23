## Install software from source packages

One way to install software in Linux is by installing directly from it source code distributed in a compressed tar ball. This might contain a setup script, sometimes it is just the source files. This requires to complie the software in order to install it. Sometimes you need to install software in this way if your platform is an uncommon one that needs binaries to be compiled in a specific way. A disadvantage is that there is no central registration on your machine of software that was installed and to check if the dependencies are there or not.

## Software packages

A package is a tar ball with a script that copy files to the right location as well as a database that tracks what was installed. Packages focus on the software they want to install and dependencies are used to related packagase such as libraries. To install a package, you need to install their dependencies, and managing dependencies is difficult. Some standard package formats are RPM for Red Hat and DEB for Ubuntu.

Libraries make working with software easier, as the libraries can be shared in multiple software packages. They provide common functionality that may be used by multiple packages. Some of them are very specific and some of them are very generic. When installing software packages, libraries are installed as dependencies if they are not present.

rpm -ivh packagetarball
ldd binarypath

## Software managers

Software managers helps to prevent dependency problems. They work with repositories where packages are stored. Before installing a package, the software manager analize dependencies and fetch them from repositories. They are provided by Linux distributions, software vendors and you can have your own. Some common software managers are `yum`, `dnf` and `apt`.

In Red Hat Linux, software managers are `yum` (old soluton) and `dnf` (newer solution). Currently both commands are using the same binary. `dnf` uses repositories defined in `/etc/yum.repos.d`. `dnf` works not only with packages but also with package groups that install several packages with related functionality, and modules, that provide a way to install different versions of packages and all their dependencies.

| Command                                      | Explanation                                                                                      |
|----------------------------------------------|--------------------------------------------------------------------------------------------------|
| `dnf install <package>`                      | Installs the specified package.                                                                  |
| `yum install -y <package>`                   | Installs the specified package without prompting for confirmation.                               |
| `dnf search <term>`                          | Searches for packages matching the specified term.                                               |
| `dnf provides */selinux`                     | Finds which package provides the specified file or capability (e.g., `selinux`).                  |
| `dnf remove <package>`                       | Removes the specified package.                                                                   |
| `dnf groups list`                            | Lists all available package groups.                                                              |
| `dnf groups install <group>`                 | Installs all packages in the specified group.                                                    |
| `yum module install container-tools`         | Installs the `container-tools` module using the `yum` package manager.                           |
| `dnf history`                                | Displays the transaction history of `dnf` operations.                                            |
| `yum info <package>`                         | Displays detailed information about the specified package.                                       |
| `yum upgrade`                                | Upgrades all installed packages to the latest available versions.                                |

In Ubuntu Linux, the software manager is `apt`. It replaces `apt-cache` and `apt-get`. Repositories are defined in `/etc/apt/sources.list`. You need to make sure that the local list of packages is updated to the most recent version of packages. If you install a package containing a service, it will restart and enable that service automatically.

| Command                            | Explanation                                                                                       |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| `apt update`                       | Updates the package lists for upgrades and new package installations.                             |
| `apt list`                         | Lists all available packages.                                                                     |
| `apt list --upgradable`            | Lists all packages that have updates available.                                                   |
| `apt search <term>`                | Searches for packages matching the specified term.                                                |
| `apt-cache search <filename>`      | Searches for packages containing files matching the specified filename.                           |
| `apt install <package>`            | Installs the specified package.                                                                   |
| `apt upgrade`                      | Upgrades all installed packages to the latest versions.                                           |
| `apt autoremove`                   | Removes unnecessary packages that were automatically installed to satisfy dependencies.           |

## RPM

The command `rpm` can be useful for queries in the RPM database and packages, such as queries on which package a file come from, queries into the package database to list contents, list configuration files in a package or show scripts that are included in a package.

| Command                                 | Explanation                                                                                       |
|-----------------------------------------|---------------------------------------------------------------------------------------------------|
| `rpm -qf /my/file`                      | Queries the package that owns the specified file.                                                 |
| `rpm -ql mypackage`                     | Lists all files installed by the specified package.                                               |
| `rpm -qpc mypackage.rpm`                | Lists all configuration files in the specified RPM package.                                       |
| `rpm -qpl mypackage.rpm`                | Lists all files contained in the specified RPM package.                                           |
| `rpm -qp --scripts mypackage.rpm`       | Displays the scripts (preinstall, postinstall, preuninstall, and postuninstall) in the specified RPM package. |
| `yumdownloader <package>`               | Downloads the specified package's RPM file without installing it.                                 |
