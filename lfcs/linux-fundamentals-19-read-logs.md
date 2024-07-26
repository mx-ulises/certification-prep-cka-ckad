## journalctl

Systemd-journald is a systemd-integrated log service. As systemd is taking care of everything, it can easily caught by systemd and forward to journald. Some distributions just use journald.

By default journald doesn't persist messaged and you can see messages only since the time you reboot your system. You can use `Storage=auto` in `/etc/systemd/journald.conf` to make it persistent by saving the logs in `/var/log/journal` directory.

| Command                                                     | Explanation                                                                                       |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `journalctl`                                                | Displays log messages from the system journal.                                                    |
| `journalctl -u sshd`                                        | Displays log messages for the `sshd` service.                                                     |
| `journalctl -f`                                             | Continuously follows (tails) the system journal logs in real time.                                |
| `journalctl --dmesg`                                        | Displays the kernel ring buffer messages, similar to `dmesg`.                                     |
| `journalctl -u crond --since yesterday --until 9:00 -p info`| Displays log messages for the `crond` service with a priority level of `info` between `yesterday` and `9:00 AM`. |
| `logger hello`                                              | Adds a message "hello" to the system log.                                                         |

## rsyslogd

Syslog is the legacy service that takes care of logging. It is implemented trought rsyslogd. It works with the following elements:
 * Facility: What rsyslogd should be logging for
 * Priority: Severity of the log event
 * Destination: Defines where the message should be written to

The file `/etc/rsyslog.conf` is the main configuration file for the rsyslogd. and define rules on what needs to be log and how based on Facility, Priority and Destination.
