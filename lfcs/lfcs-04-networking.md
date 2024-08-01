## Managing configuration for network interfaces

ip link show
| Command                                 | Explanation                                                                                       |
|-----------------------------------------|---------------------------------------------------------------------------------------------------|
| `ip link show`                          | Displays information about all network interfaces.                                                |
| `lshw -class network`                   | Lists detailed information about all network hardware on the system.                              |
| `ip addr add <ip_with_mask>`            | Adds an IP address with a subnet mask to the default network interface.                           |
| `ip addr add dev <device> <ip_with_mask>` | Adds an IP address with a subnet mask to the specified network interface.                         |
| `ethtool <device_name>`                 | Displays or changes settings of the specified network device.                                     |

NetworkManager is used for persistent intrface configuration in Red Hat and indirectly on Ubuntu. Ubuntu uses Netplan and has yaml in `/etc/netplan/*.yaml` configuration abstractions along with `systemd-networkd`.

| Command                             | Explanation                                                                                       |
|-------------------------------------|---------------------------------------------------------------------------------------------------|
| `nmtui`                             | Opens a text user interface for NetworkManager to configure network settings.                     |
| `nmcli connection edit <device>`    | Opens the NetworkManager command-line interface to edit the connection settings for the specified device. |
| `man nmcli-examples`                | Displays the manual page with examples for using `nmcli`.                                         |
| `netplan apply`                     | Applies the network configuration changes defined in Netplan configuration files.                 |
| `networkctl status`                 | Shows the status of the network links managed by systemd-networkd.                                |
| `networkctl up`                     | Brings up all network links managed by systemd-networkd.                                          |
| `networkctl down`                   | Brings down all network links managed by systemd-networkd.                                        |

## Managing static routes

All networked nodes are configured with a default gateway, and it specifies which node to use to address external nodes. You can add static routes to define a route to a network that is not behind the default gateway. In Ubuntu it is used netplan yaml files.

| Command                                                            | Explanation                                                                                       |
|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `ip route show`                                                    | Displays the current routing table.                                                               |
| `ip route add <ip_range> via <ip>`                                 | Adds a new route to the specified IP range via the given IP address.                              |
| `nmcli connection modify <device> +ipv4.route "<ip_range> <ip>"`   | Adds a new IPv4 route to the specified device using NetworkManager's command-line interface.      |
| `nmcli con down <device>`                                          | Brings down the specified network connection.                                                     |
| `nmcli con up <device>`                                            | Brings up the specified network connection.                                                       |

## Managing hostnames

DNS is commonly used for hostname resolving. Locally you have a DNS configuration file in `/etc/hosts` for domain resolution. The file `/etc/nsswitch.conf` help to define rules to resolve the hostnames (the name service switch).

The local hostname is known to the kernel trohuyh `/proc/sys/kernel/hostname`. In modern distributions it is managed with `hostnamectl`.

| Command                                   | Explanation                                                                                       |
|-------------------------------------------|---------------------------------------------------------------------------------------------------|
| `hostnamectl status`                      | Shows the current hostname and related system information.                                        |
| `hostnamectl set-hostname <new_hostname>` | Sets the system's hostname to the specified new hostname.                                         |
