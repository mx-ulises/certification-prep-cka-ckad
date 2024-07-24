## Managing Linux time

The linux time has hardware time that is loaded at boot time, then the OS set it's own time based on hardware time. OS time can be synchronzied either by hardware time or with external services via the Network Time Protocol (NTP)

| Command                                    | Explanation                                                                                       |
|--------------------------------------------|---------------------------------------------------------------------------------------------------|
| `date`                                     | Displays the current date and time.                                                               |
| `date +%d-%m-%Y`                           | Displays the current date in the format `day-month-year`.                                         |
| `hwclock`                                  | Displays the hardware clock time.                                                                 |
| `hwclock --systohc`                        | Sets the hardware clock to the current system time.                                               |
| `timedatectl`                              | Displays the current system time, date, and timezone settings.                                    |
| `timedatectl show`                         | Displays current settings of the `timedatectl` properties.                                        |
| `timedatectl list-timezones`               | Lists all available timezones.                                                                    |
| `timedatectl set-timezone Africa/Banjul`   | Sets the system timezone to `Africa/Banjul`.                                                      |

## NTP protocol and configure time synchronization

Network Time Protocol (NTP) is an important protocol in Linux, as it helps to manage the system time (systime). Some common NTP servers are chrony and timesyncd and they get the time from the internet. NTP uses the notion of stratum to determine different reliability levels of the time from 16 (less reliable) to 1 (most reliable). NTP servers will uses different sources of time with different stratums, and it will syncronize with the most reliable.

| Command                            | Explanation                                                                                       |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| `timedatectl timesync-status`      | Displays the status of the time synchronization service.                                          |
| `timedatectl status`               | Shows the current system time, date, and timezone settings along with synchronization status.     |
| `systemctl status chronyd`         | Displays the status of the `chronyd` service, which handles time synchronization.                 |
| `timedatectl set-ntp <bool>`       | Enables or disables Network Time Protocol (NTP) synchronization. (`<bool>` should be `true` or `false`)|
| `chronyc sources`                  | Shows information about the current time sources that `chronyd` is using.                         |
| `chronyc tracking`                 | Displays detailed information about the current time and synchronization status.                  |
| `ntpq`                             | Queries the NTP daemon about its status, peers, and more.                                         |
| `ntpdate <server>`                 | Sets the system clock by querying the specified NTP server.                                       |
