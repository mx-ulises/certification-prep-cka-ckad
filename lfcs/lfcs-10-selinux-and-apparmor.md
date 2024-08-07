## Mandatory Access Control

The main solutions for Mandatory Access Control are SELinux (Red Hat) and AppArmor (Ubuntu).

 * SELinux:
   * Locks down everything, if it isn't allowed, then it is denied
   * Complex to learn
   * Offer advanced features such as multi-level security
   * Filesystem labels
 * AppArmor:
   * Works with profiles to secure specific services
   * Easy to learn

## AppArmor

It needs to be enabled with systemd, it has an application profile in `/etc/apparmor.d`, loaded profiles are in `/sys/kernel/security/apparmor`. AppArmor is focusing on specific applications, so similar applications will need to create similar profiles separately.

| Command                                                                  | Explanation                                                                                       |
|--------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl status apparmor`                                              | Displays the current status of the AppArmor service.                                              |
| `apt search apparmor`                                                    | Searches for AppArmor packages available in the APT repositories.                                 |
| `apt install apparmor-profiles apparmor-profiles-extra apparmor-utils`   | Installs additional AppArmor profiles and utilities.                                              |
| `aa-genprof /your/application`                                           | Generates a new AppArmor profile for the specified application.                                   |
| `aa-status`                                                              | Displays the current status of AppArmor, including loaded profiles and their modes.               |

## SELinux

It provides a policy that identifies which source object has access to which target object. The rules are based on labels consisting on three parts: User, Role and Type. For SELinux use, the context type is used, when a specific source context type requests access to a specific target context type, SELinux checks if there is a rule that allows this, if not, it is going to deny.

SELinux is either enable or disabled and this is an state in the kernel, and can only be set while booting (`selinux=0` or `selinux=1`). If it is enabled, you can toggle between permissive and enforcing. Permissive mode, nothing is blocked but all events are logged, in enforcing mode, SELinux is fully functional. It store persistend configuration in `/etc/sysconfig/selinux`

| Command                     | Explanation                                                                                       |
|-----------------------------|---------------------------------------------------------------------------------------------------|
| `getenforce`                | Displays the current mode of SELinux (Enforcing, Permissive, or Disabled).                        |
| `setenforce permissive`     | Temporarily sets SELinux to permissive mode, where violations are logged but not enforced.        |
| `setenforce enforcing`      | Temporarily sets SELinux to enforcing mode, where violations are enforced and logged.             |

All items are using context labels, and you can use `-Z` to get label information.

| Command                                                                 | Explanation                                                                                                  |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| `semanage fcontext`                                                     | Manages SELinux file context definitions.                                                                    |
| `restorecon`                                                            | Restores the default SELinux context for files.                                                              |
| `semanage port`                                                         | Manages SELinux port context definitions.                                                                    |
| `ps Zaux`                                                               | Displays process status with SELinux security context.                                                       |
| `ls -Z <directory>`                                                     | Lists files in a directory with their SELinux security context.                                              |
| `grep AVC /var/log/audit/audit.log`                                     | Searches for Access Vector Cache (AVC) denials in the audit log.                                             |
| `man semanage-fcontext`                                                 | Displays the manual page for managing SELinux file context definitions.                                      |
| `semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"`              | Adds a new SELinux file context rule for the specified directory and its contents.                           |
| `restorecon -Rv <directory>`                                            | Recursively restores the default SELinux context for files in the specified directory, showing verbose output.|
| `man semanage-port`                                                     | Displays the manual page for managing SELinux port context definitions.                                      |
| `semanage port -a -t httpd_port_t -p tcp 82`                            | Adds a new SELinux port context rule for HTTP traffic on TCP port 82.                                        |

A boolean is SELinux is an on/off switch that allows to apply setting in SELinux. They are used in addition to context labels.

| Command                           | Explanation                                                                                      |
|-----------------------------------|--------------------------------------------------------------------------------------------------|
| `getsebool -a`                    | Displays the current status of all SELinux booleans.                                             |
| `setsebool -P tftp_anon_write on` | Permanently enables the SELinux boolean that allows anonymous users to write to TFTP directories.|

SELinux are logged to `/var/log/audit/audit.log`, that are not very readable. There are other tool `sealert` that logs readable messaged to the logging system, this will provide commands to run for troubleshooting.

| Command                               | Explanation                                                                              |
|---------------------------------------|------------------------------------------------------------------------------------------|
| `journalctl | grep sealert`           | Searches the systemd journal logs for SELinux alert messages.                            |
| `sealert -l <label>`                  | Displays detailed information about a specific SELinux alert identified by the given label. |
