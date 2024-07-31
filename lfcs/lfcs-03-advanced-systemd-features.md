## Systemd units

| Command                               | Explanation                                                                                       |
|---------------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl show unit.type`            | Shows properties of the specified `unit.type` (e.g., service, socket).                            |
| `man systemd.directives`              | Displays the manual page for `systemd` directives, listing configuration directives and their descriptions. |
| `systemctl cat unit.type`             | Displays the content of the specified unit file.                                                  |
| `systemctl edit unit.type`            | Opens the specified unit file for editing, allowing you to override or extend the unit configuration. |
| `systemctl daemon-reload`             | Reloads the systemd manager configuration, necessary after modifying unit files.                  |
| `systemctl restart unit.type`         | Restarts the specified unit.                                                                      |
| `systemctl status unit.type`          | Shows the status of the specified unit, including whether it is active, its logs, and any errors.  |

## Systemd sockets

A systemc socket is used with a service to start the service when traffic comes in on a specific socket. The service and socket files need to have the same name. Use `ListenStream` to listen on a TCP port. Use `ListenDatagram` to listen on a UDP port. When using sockets, the socket is enabled and not the service.

| Command                                     | Explanation                                                                                       |
|---------------------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl list-unit-files -t socket`       | Lists all unit files of type `socket`.                                                            |
| `systemctl cat cockpit.socket`              | Displays the content of the `cockpit.socket` unit file.                                           |
| `systemctl status cockpit.service`          | Shows the status of the `cockpit.service` unit.                                                   |
| `systemctl enable --now cockpit.socket`     | Enables and starts the `cockpit.socket` unit immediately.                                         |

## Systemd timers

A systemd timer is used to start the corresponding service at a specific time or event. The timer and service need to have the same filename. Use `OnCalendar` to specify when the timer should be started in a cron-like style. Use `OnBootSec` or `OnUnitActiveSec` to run based on other events. When usingtimers. the timer is enabled/started, not the service.

| Command                                     | Explanation                                                                                       |
|---------------------------------------------|---------------------------------------------------------------------------------------------------|
| `man systemd.time`                          | Displays the manual page for `systemd.time`, detailing time and date specifications for systemd.  |
| `systemctl list-unit-files -t timer`        | Lists all unit files of type `timer`.                                                             |
| `systemctl cat fstrim.timer`                | Displays the content of the `fstrim.timer` unit file.                                             |
| `man systemd.timer`                         | Displays the manual page for `systemd.timer`, detailing timer unit configuration and usage.       |
| `systemctl enable --now fstrim.timer`       | Enables and starts the `fstrim.timer` unit immediately.                                           |

## Systemd cgroups

The cgroups are related to place resourcs in controllers that represent the type of resourcers. Common default controllers are `cpu`, `memory` and `blkio`. Thes controllers are subdivided in a tree structure where different weights or limits are applied to each branch:
 * each of these branches is a cgroup
 * One or more proccesses are assigned to a cgroup

Cgroups can be appliedfrom the command line or from systemd. Manual creation happened trought the cgconfig service and the cgred process. In all cases, cgroup settins are written to `/sys/fs/cgroups`. Systemd dives crontrollers into slices:
 * `system` for system processes and daemons
 * `machine` for virtual machine
 * `user` for user sessions
 * Custom slices can be created as well.

| Command              | Explanation                                                                                       |
|----------------------|---------------------------------------------------------------------------------------------------|
| `systemd-cgtop`      | Displays a top-like view of the system's control groups (cgroups) and their resource usage.       |
| `systemd-cgls`       | Lists the control groups (cgroups) and their hierarchical structure.                             |

## Systemd unit dependencies

Different statements can be used in the `[Unit]` section to define unit file dependencies:
 * `before`: This unit starts before the specified unit
 * `after`: This unit starts when the specified unit is started.
 * `requires`: When this unit starts, the unit listed here is also started. If thespecified nit fails, this one also fail.
 * `wanted`: When this unit starts, the unit listed here also start

| Command                           | Explanation                                                                                       |
|-----------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl status httpd`          | Shows the status of the `httpd` service, including whether it is active, its logs, and any errors.|
| `systemctl status vsftpd`         | Shows the status of the `vsftpd` service, including whether it is active, its logs, and any errors.|
| `systemctl edit httpd.service`    | Opens the `httpd.service` unit file for editing, allowing you to override or extend the unit configuration. |
| `systemctl start httpd`           | Starts the `httpd` service.                                                                       |

## Systemd self-healing

| Command                          | Explanation                                                                                       |
|----------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl edit sshd.service`    | Opens the `sshd.service` unit file for editing, allowing you to override or extend the unit configuration. |
| `systemctl daemon-reload`        | Reloads the systemd manager configuration, necessary after modifying unit files.                  |
| `systemctl status sshd`          | Shows the status of the `sshd` service, including whether it is active, its logs, and any errors.  |
