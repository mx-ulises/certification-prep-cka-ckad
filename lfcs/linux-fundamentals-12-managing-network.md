## IPv4 basics

An IP is composed of 4 bytes to identify devices. Eg:

```
192.168.1.12/24
```

The `/24` represents the network part of the IP address, in this case is the last 3 bytes (24 bits) of the address (`192.168.1`). Usually routers use the last digit (`254`) to identify themselves.

A Domain Name Server (DNS) to resolve names instead of ip addresses.

## IPv6 basics

IPv6 128 bits are ussed for express address, usually expresed in hexadecimal. leading zeros and only-zero strings can be ommited. To refer to an addres + port, you sue the following format:

```
[address]:port
```

The subnet mask is also used in IPv6 and it is usually `/64`. A node bit of the addrsss may contain the device MAC address. Reserved IPv6 addresses:

 * `::1/128`: Localhost
 * `::`: unspecified address (`0.0.0.0`)
 * `::/0`: default route
 * `200::/3`: Global unicast address. This is a worldwide unique address and it is reserving the address with `2000` prefix.
 * `fd00::/8`: Unique local address (private address)
 * `fe80::/64`: Link-local address
 * `ff00::/8`: multicast

By default, `fe80::` address is assigned. You can also get an static address as well.

## Runtime network configurations

On servers, IP address configuration is set statically. On workstations, IP address configuration tipically is handed out by a DHCP sever. For troubleshooting pirposes, ip command is used to set runtime network configurations that are not persistent.

| Command                                      | Explanation                                                                                       |
|----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `nmcli`                                      | Command-line tool for managing NetworkManager and network connections.                            |
| `nmtui`                                      | Text user interface for managing NetworkManager and network connections.                          |
| `ip`                                         | Utility for configuring network interfaces, routing, and tunnels.                                 |
| `ip addr show`                               | Displays all IP addresses assigned to all network interfaces.                                     |
| `ip addr add dev ens160 10.0.0.10/24`        | Adds the IP address `10.0.0.10` with a subnet mask of `24` to the network interface `ens160`.      |
| `ip link set ens160 up`                      | Activates the network interface `ens160`.                                                         |
| `ip link set ens160 down`                    | Deactivates the network interface `ens160`.                                                       |
| `ip route show`                              | Displays the routing table.                                                                       |
| `ip route add default via 10.0.0.1`          | Adds a default gateway with the IP address `10.0.0.1`.                                            |

## Network device naming

In old linux, network device names were named according to the type of network device: `eth0` or `wlan0`. In modern linux, network devices are named according to their physical location in the computer, with the help of the driver. Biosdevname uses device names that reveal information about their fisical location, and `systemd-udevd` generates the network device names:
 * `em123`: ethernet motherboard portnumber
 * `p<port>p<slot>`: PCI and PCI port
 * `eno123`: EtherNet Oboard
 * If driver doesnt reveal sufficient informaiton, `eth0` is used

## Manage host names and host names resolution

For correct operations, it is important that Linux hosts have the right name set. Also, host name resolution is required, as often reversed host name lookups are performed in communication between hosts. It can be also configured the `/etc/hosts` file with appropiate host name lookup settings.

| Command                             | Explanation                                                                                       |
|-------------------------------------|---------------------------------------------------------------------------------------------------|
| `hostname`                          | Displays the current system hostname.                                                             |
| `hostnamectl`                       | Command-line utility to control the system hostname and related settings.                         |
| `hostnamectl status`                | Displays detailed information about the system's hostname and related settings.                   |
| `hostnamectl set-hostname newhostname` | Sets the system's hostname to `newhostname`.                                                     |

Hostname resolution allows host names to be matched to IP addresses and viceversa. Some available configurations are:

 * `/etc/hosts` as primary solution
 * `/etc/resolv.conf` for DNS configuration
 * `/etc/nsswitch.conf` to configure the order of host name resolution mechanisms

## Common network tools

| Command                            | Explanation                                                                                       |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| `ping`                             | Sends ICMP ECHO_REQUEST packets to test network connectivity and measure round-trip time.         |
| `ping <server>`                    | Pings a specified server to test connectivity and measure round-trip time.                        |
| `ping -c 3 <server>`               | Pings a specified server 3 times and then stops.                                                  |
| `ss`                               | Investigates sockets, displaying detailed network socket information.                             |
| `ss -tunap`                        | Displays all TCP, UDP, and raw sockets with process information.                                  |
| `dig google.com`                   | Queries DNS for information about the domain `google.com`.                                        |
| `nmap -sn 192.168.29.0/24`         | Performs a ping scan on the specified network range to discover active hosts.                     |
| `nmap <server>`                    | Scans the specified server to discover open ports and services.                                   |
| `netstat` (deprecated)             | Displays network connections, routing tables, interface statistics, masquerade connections, and multicast memberships. (Deprecated in favor of `ss`) |
| `nslookup` (deprecated)            | Queries DNS to obtain domain name or IP address mapping. (Deprecated in favor of `dig`)            |

## Configuring persistent networking

If you change an IP address, it wount be kept if you do with `ip`. NetworkManager is the common service to takes care of persistent networking by writing the configuration in persistent files.

| Command  | Explanation                                                                                       |
|----------|---------------------------------------------------------------------------------------------------|
| `nmtui`  | Text user interface for managing NetworkManager and network connections.                          |
| `nmcli`  | Command-line tool for managing NetworkManager and network connections.                            |
