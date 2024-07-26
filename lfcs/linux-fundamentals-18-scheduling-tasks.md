## Scheduling with cron

This is the classical solution to schedule re-ocurring tasks. It uses the `crond` daemon and `crontab -e` to edit tasks. This is used as a generic solution.

| Command                       | Explanation                                                                                       |
|-------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl status crond`      | Displays the status of the `crond` service, which handles scheduled tasks (cron jobs).            |
| `vim /etc/crontab`            | Opens the system-wide crontab file in the `vim` text editor for editing.                          |
| `crontab -e`                  | Opens the user's crontab file in the default text editor for editing.                             |
| `ls /etc/cron.*`              | Lists the contents of the directories related to cron jobs, such as `cron.d`, `cron.daily`, `cron.hourly`, `cron.monthly`, and `cron.weekly`. |

## Systemd timers

Systemd timers is the new alternative. It works by creating a `.timer` unit and run it using `systemctl`. This is the future of task scheduling and a replacement for `cron`.

| Command                                      | Explanation                                                                                       |
|----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl list-units -t timers`             | Lists all active timer units in the system.                                                       |
| `systemctl list-unit-files -t timer`         | Lists all timer unit files installed on the system.                                               |
| `systemctl list-unit-files fstrim*`          | Lists all unit files related to `fstrim`.                                                         |
| `systemctl cat fstrim.timer`                 | Displays the contents of the `fstrim.timer` unit file.                                            |

## Scheduling with at

This is used to run tasks that only need to run once. It uses `atd` daemon and the command `at` to schedule tasks. This is to run a single task, but not that common.

| Command                   | Explanation                                                                                       |
|---------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl status atd`    | Displays the status of the `atd` service, which handles scheduled one-time tasks.                 |
| `at teatime`              | Schedules a command to run at teatime (usually interpreted as 4 PM).                              |
| `at 10:20`                | Schedules a command to run at 10:20 AM.                                                           |
| `atq`                     | Lists the user's pending scheduled `at` jobs.                                                     |
| `atrm 1`                  | Removes the scheduled `at` job with job number 1.                                                 |
