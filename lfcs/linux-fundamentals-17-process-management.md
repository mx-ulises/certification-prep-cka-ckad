## Proceccess and jobs

Anything that runs on linux, runs as a proccess. Proceccess has a process ID (PID) and can be managed by sending signals to them. Signals are operating system intructions to proccesses. Some signals are:

 * `SIGTERM` (`15`): Is the normal signal to instruct a proccess to stop
 * `SIGKILL` (`9`): Is the signal to force a process to stop immediately

Jobs are user processes that have been started from a specific shell. User can manage their own jobs. All processes originate as child from systemd. Some kernel processes are listed as children of systemd. Procceses you run in the shell goes as children of bash procceses.

## Managing interactive shell jobs

When a user run a command, this command is started as a foreground job. While run is running, you cannot use your console. You can move a running procces to the background. With `Ctrl+z` you can stop jobs temporary and `Ctrl+c` to stop it permanently.

| Command      | Explanation                                                                                        |
|--------------|----------------------------------------------------------------------------------------------------|
| `jobs`       | Lists all background and suspended jobs in the current shell session.                              |
| `fg`         | Brings the most recently suspended or background job to the foreground.                            |
| `fg 2`       | Brings the job with job number 2 to the foreground.                                                |
| `bg`         | Resumes a suspended job and runs it in the background.                                             |

## Top

To manage procceses as administrator, you can use `top`, that list procceses by CPU utilization. It provides details about the job susch as PID, user, command, resource utilization, etc... As well as health metrics such as load average, status of proccesses, etc...

| Command  | Explanation                                                                                        |
|----------|----------------------------------------------------------------------------------------------------|
| `top`    | Displays real-time information about system processes, including CPU and memory usage.             |

To change fields in `top` you can use `f` to list the properties that can be listed. Command `w` save the settings configuration for your active user.

## PS

The command `ps` will provide you information about the procceses. Some other properties help to show more properties or filter procceses, and using `awk`, `sort` or `grep` is useful along with `ps`.

| Command                      | Explanation                                                                                           |
|------------------------------|-------------------------------------------------------------------------------------------------------|
| `ps`                         | Displays a snapshot of the current processes.                                                         |
| `ps aux`                     | Displays detailed information about all running processes, including those not owned by the user.     |
| `ps fax`                     | Displays a tree view of processes, showing the hierarchical relationships.                            |
| `ps -ef`                     | Displays all running processes in a full-format listing.                                              |
| `ps -e -o pid,args --forest` | Displays all running processes in a tree format, showing only the PID and arguments.                  |
| `ps aux --sort pmem`         | Displays all running processes, sorted by memory usage.                                               |

## Managing processes

By default, all user procceses starts with the same priprity. To adjust priorities, you can use `nice`. Non-priviledge uses can only run processes with a lower priority. Priviledge users can increase an decrease process priority. Priorities reach from -20 (highest priority) to 19 (lowest priority). To adjust priorities you can use `renice` or `top`.

| Command                                  | Explanation                                                                                       |
|------------------------------------------|---------------------------------------------------------------------------------------------------|
| `nice`                                   | Runs a command with a modified scheduling priority (default is 10).                               |
| `nice -n -10 dd if=/dev/zero of=/dev/null` | Runs the `dd` command with a higher priority (negative nice values increase priority).            |
| `renice`                                 | Alters the priority of a running process.                                                         |

The `kill` command is used to send signals to a proccess, based on the PID. The command `killall` do the same but based in the process name. Common signals are:
 * `SIGTERM` (`15`): Is the normal signal to instruct a proccess to stop
 * `SIGKILL` (`9`): Is the signal to force a process to stop immediately

| Command                     | Explanation                                                                                       |
|-----------------------------|---------------------------------------------------------------------------------------------------|
| `kill pid`                  | Sends a signal (default is TERM) to the specified process by its PID, terminating the process.    |
| `killall processname`       | Sends a signal (default is TERM) to all processes with the specified name, terminating them.      |
