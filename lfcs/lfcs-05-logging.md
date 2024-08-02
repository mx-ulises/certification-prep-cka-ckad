## Logging

Most Linux distribution uses systemd-journald and rsyslogd. Some other services have their own logging system. Systemd-journald is part of systemd so it is easy to capture messages from systemd units. The rsyslogd is compatible with the legacy syslogd, and facilitate to write messages in appropiate locations. Messages are writen in `/var/log` in rsyslogd.

| Command                           | Explanation                                                                                       |
|-----------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl status cron.service`   | Shows the status of the `cron` service, including whether it is active, its logs, and any errors. |
| `journalctl`                      | Displays logs from `systemd` journal.                                                             |
| `journalctl -u cron.service`      | Displays logs specifically related to the `cron` service.                                         |
| `ls -l /var/log`                  | Lists the files in the `/var/log` directory with detailed information.                            |
| `less /var/log/syslog`            | Views the contents of the `syslog` file with `less` for easy navigation.                          |

## Journald

Usually, systemd journal is not set to be persistent. You can check in `/etc/systemd/journald.conf` with option `Sotrage=auto`, and a persistent journal will be created after creating a directory `/var/log/journal`.

| Command                                           | Explanation                                                                                       |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl restart systemd-journal-flush.service` | Restarts the `systemd-journal-flush` service, which ensures all journal data is written to disk.  |
| `vim /etc/systemd/journald.conf`                  | Opens the `systemd-journald` configuration file for editing using `vim`.                          |
| `mkdir /var/log/journal`                          | Creates a persistent directory for `systemd` journal logs.                                        |

## Rsyslogd

In rsyslogd, facilities and priorities are used to define how logging should happen. Facilities define the item for which logging is happening. Priorities define the severity levels in which case messages should be logged, and modules can be used to handling log's input and output.

| Command                             | Explanation                                                                                       |
|-------------------------------------|---------------------------------------------------------------------------------------------------|
| `vim /etc/rsyslog.conf`             | Opens the `rsyslog` configuration file for editing using `vim`.                                   |
| `systemctl restart rsyslog.service` | Restarts the `rsyslog` service to apply any configuration changes.                                |

## Logrotate

Logrotate is a mechanism that ensures that log diles dont grow too big. It is started as a scheduled job thorught cron or systemd timers. They are configured in `/etc/logrotate.conf` and `/etc/logrotate.d/*` files.

| Command                                                       | Explanation                                                                                       |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `systemctl list-units -t timer \| grep logrotate.timer`       | Lists all timer units and filters the output to show only `logrotate.timer`.                      |
| `systemctl cat logrotate.timer`                               | Displays the content of the `logrotate.timer` unit file.                                          |
