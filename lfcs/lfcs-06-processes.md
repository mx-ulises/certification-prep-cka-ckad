## Resource limits

You can set runtime limits with `ulimit`. It applies to the shell whichthe command is used. Appy persistent ulimit in `/etc/security/limits.conf`. Soft limits can be modified by the users, and hard limits implement an absolute ceiling, meaning that you can set soft limits lower than the hard limits.

| Command     | Explanation                                                                                       |
|-------------|---------------------------------------------------------------------------------------------------|
| `ulimit -a` | Displays all the current user limits for the shell session.                                        |
| `man bash`  | Opens the manual page for the `bash` shell, detailing its usage and options.                      |

You can use persistent limits with `pam_limits`. It works with  `/etc/security/limits.conf` and  `/etc/security/limits.d/*.conf`.

| Command              | Explanation                                                                                       |
|----------------------|---------------------------------------------------------------------------------------------------|
| `man 5 limits.conf`  | Opens the manual page for `limits.conf`, which describes the format and options for configuring resource limits in `/etc/security/limits.conf`. |

You can also use systemd units. Add the `Limit*` entries to the `[Service]` block of a unit file to limit what services can do.

| Command                     | Explanation                                                                                       |
|-----------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl show <unit>`     | Displays detailed information about the specified systemd unit.                                   |
| `systemctl daemon-reload`   | Reloads the systemd manager configuration, necessary after modifying unit files.                  |
| `systemctl restart <unit>`  | Restarts the specified systemd unit.                                                              |
| `man 5 systemd.exec`        | Opens the manual page for `systemd.exec`, detailing execution environment configuration for systemd services. |
| `man 2 setrlimit`           | Opens the manual page for the `setrlimit` system call, which is used to get and set resource limits.|

Cgroups place resources in controllers that represent the type of resource. By default, each proccess get equal ammounts. Cgroups are used to limit the availability of resources using limits and weights and they are integrated in systemd.

## Managing IPC

System V IPCs are the old way to handle communication between processes. In modern Linux there are other methods available such as RPC (Remote Procedure Calls) and D-Bus.

IPC enables communication in user space:
 * Shared files: Two programs read and write to shared file
 * Shared memoru with semaphores: values in memory that can be tested and set by multiple programs
 * Named and unnamed pipes: The output of one program is used as the input of another.
 * Message queues: One process writes to the queue, multiple processes can access
 * Sockets: Local or network port used for communication
 * Signals: Generated to alert processes.

| Command             | Explanation                                                                                       |
|---------------------|---------------------------------------------------------------------------------------------------|
| `ipcs`              | Displays information on inter-process communication facilities (shared memory, message queues, semaphores). |
| `ipcs -p`           | Displays the creator and last process that operated on the IPC facilities.                        |
| `lsipc`             | Lists information about all IPC facilities on the system.                                         |
| `top`               | Displays a real-time, dynamic view of system processes.                                           |
| `mkfifo myfifo`     | Creates a named pipe (FIFO) called `myfifo`.                                                      |
| `cat myfifo`        | Reads data from the named pipe `myfifo`.                                                          |
| `cat > myfifo`      | Writes data to the named pipe `myfifo`.                                                           |
| `unlink myfifo`     | Deletes the named pipe `myfifo`.                                                                  |

D-Bus is a modern way of IPC. It provides a software bus abstraction to which all processes connect. It provides a virtual channel to not require point-to-point communication and multiple busses can be used. Applications should be D-Bus aware so they can expose uniform interfaces. D-Bus is managed by API usually.

| Command          | Explanation                                                                                       |
|------------------|---------------------------------------------------------------------------------------------------|
| `busctl list`    | Lists all the services currently available on the D-Bus message bus.                              |
| `dbus-send`      | Sends a message to a D-Bus service, used for communicating with D-Bus enabled services.           |

## Managing OOM

If Linux run out of memory, the Out of Memory (OOM) killer becomes active and kills the process with the highest OOM score. This ensures that memory is available to run new process. OOM logs should be observed when processes are killed by OOM. To test what happens in OOM situations, you can use `/proc/sysrq-trigger`. This might not be available in all Linux systems, so it need to be enable.

| Command                                       | Explanation                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `man proc`                                    | Opens the manual page for the `/proc` filesystem, which contains information about system processes and kernel. |
| `echo 1 > /proc/sys/kernel/sysrq`             | Enables the magic SysRq key, allowing certain low-level commands to be sent directly to the kernel.|
| `echo h > /proc/sysrq-trigger`                | Triggers the magic SysRq key to display help information on available commands.                   |
| `dmesg`                                       | Displays the kernel ring buffer messages, useful for debugging hardware and kernel issues.        |
| `echo f > /proc/sysrq-trigger`                | Triggers the magic SysRq key to force a crash and generate a memory dump for debugging.           |
| `journalctl | grep -A 10 "Out of memory"`      | Searches the system journal for "Out of memory" messages and displays 10 lines following each match.|

## I/O monitoring and tuning

| Command                                           | Explanation                                                                                       |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `top`                                             | Displays a real-time, dynamic view of system processes.                                           |
| `iotop`                                           | Shows I/O usage by processes or threads on the system in a top-like interface.                    |
| `echo 1 > /proc/sys/kernel/task_delayacct`        | Enables task delay accounting, which collects information about delays in task execution.         |
| `vmstat 2 10`                                     | Reports virtual memory statistics every 2 seconds for 10 iterations.                              |
| `echo none > /sys/block/sda/queue/scheduler`      | Sets the I/O scheduler for the specified block device (e.g., `sda`) to 'none'.                    |
