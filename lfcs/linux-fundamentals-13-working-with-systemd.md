## Managing Systemd services

Systemd is the manager of everything in Linux.
 * It is the first thing that is started after the Linux Kernel. It start procceses in parallel, as well as managing mounts, timers, paths and more stuff.
 * Systemd is event driven reacting to specific events.
 * Items managed by Systemd are called units:
   * `/usr/lib/systemd/system` contains the default units
   * `/etc/systemd/system` contains the custom units

Systemd units can be managed with `systemctl`, including modifications on the configuration of the services.

| Command                         | Explanation                                                                                      |
|---------------------------------|--------------------------------------------------------------------------------------------------|
| `systemctl -t help`              | Lists all available unit types in `systemctl`.                                                   |
| `systemctl list-unit-files`      | Lists all unit files and their current enablement status.                                        |
| `systemctl list-units`           | Lists all currently active units, including services, sockets, and other unit types.             |
| `systemctl enable name.service`  | Enables the specified service to start automatically at boot.                                     |
| `systemctl start name.service`   | Starts the specified service immediately.                                                         |
| `systemctl disable name.service` | Disables the specified service from starting automatically at boot.                               |
| `systemctl stop name.service`    | Stops the specified service immediately.                                                          |
| `systemctl status name.service`  | Displays the current status and recent log entries of the specified service.                      |
| `systemctl cat name.service`     | Displays the content of the specified service's unit file.                                        |
| `systemctl show`                 | Displays properties of the specified unit.                                                        |
| `systemctl edit name.service`    | Opens the specified service's unit file in a text editor for editing.                             |
| `systemctl daemon-reload`        | Reloads the systemd manager configuration.                                                        |
| `systemctl restart name.service` | Restarts the specified service.                                                                   |

## Managing targets

A target is a group of services. Some targets can be used in isolation, which means that you can use them to define the state your system should be started in:
 * emergency.target
 * rescue.target
 * multi-user.target
 * graphical.target

| Command                               | Explanation                                                                                      |
|---------------------------------------|--------------------------------------------------------------------------------------------------|
| `systemctl list-dependencies`         | Lists dependencies for all units.                                                                |
| `systemctl list-dependencies name.target` | Lists dependencies for the specified target.                                                  |
| `systemctl get-default`               | Displays the default target that the system boots into.                                          |
| `systemctl set-default name.target`   | Sets the specified target as the default target that the system boots into.                      |
| `systemctl list-units`                | Lists all currently active units, including services, sockets, and other unit types.             |
| `systemctl list-units -t target`      | Lists all currently active target units.                                                         |
| `systemctl start name.target`         | Starts the specified target, bringing the system into that target state.                         |
| `systemctl isolate name.target`       | Isolates the specified target, stopping all units not needed by that target.                     |
