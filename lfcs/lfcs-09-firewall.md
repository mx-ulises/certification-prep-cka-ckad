## Firewall

The Kernel has a functionality called netfilter that is basically a packet filter. There are three directions that the package can take: input, forwarded and output. Fireware rules are written using firewalld and ufw that writes directly iptables or nftables and they define rules on the different directions.

## Firewalld

 * Zone: a collection of network cards that is facing a specific direction to which rules can be assigned
 * Interfaces: individual network cards, assigned to a zone
 * Services: An xml-based configuration that specifies ports to be opened and modules that should be used. These are kernel modules
 * Forward ports: used to send traffic coming in on a specific port to another port which migth be in another server.
 * Masquereding: Provide Network Address Translation on a router.
 * Rich rules: Extensions to the firewalld syntax to make more complex configurations possible.

| Command                                      | Explanation                                                                                       |
|----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `firewall-cmd`                               | Command-line interface for managing firewalld, a firewall management tool.                        |
| `firewall-cmd --help`                        | Displays help information and usage details for `firewall-cmd`.                                   |
| `firewall-cmd --list-services`               | Lists all services currently allowed by the firewall.                                             |
| `firewall-cmd --get-services`                | Lists all predefined services recognized by firewalld.                                            |
| `firewall-cmd --add-service service`         | Adds the specified service to the firewall rules.                                                 |
| `firewall-cmd --remove-service service`      | Removes the specified service from the firewall rules.                                            |
| `firewall-cmd --add-port 123/tcp`            | Opens the specified port (123) with the specified protocol (TCP) in the firewall.                 |
| `firewall-cmd --list-all`                    | Lists all the current settings and rules of the firewall.                                         |
| `firewall-cmd --runtime-to-permanent`        | Saves the current runtime configuration to the permanent configuration.                           |
| `cat /usr/lib/firewalld/services/service.xml`| Displays the contents of the specified service's XML configuration file.                          |
| `systemctl restart firewalld`                | Restarts the firewalld service to apply any changes.                                              |

Network interfaces in firewalld are assigned to a zone, with the default zone being default. On multi-homed servers, using multiple zones is usual to ensure specific traffic types are allowed in specific environments only.

| Command                                            | Explanation                                                                                       |
|----------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `firewall-cmd --get-zones`                         | Lists all available zones in firewalld.                                                           |
| `firewall-cmd --list-all-zones`                    | Lists all zones and their configurations.                                                         |
| `firewall-cmd --get-default-zone`                  | Displays the current default zone.                                                                |
| `firewall-cmd --set-default-zone zone`             | Sets the specified zone as the default zone.                                                      |
| `firewall-cmd --add-service service --zone zone`   | Adds the specified service to the specified zone.                                                 |

Rich rules use an expressive language to create custom rules that cannot be created with basic syntax. Some features includes:

 * logging
 * port forwarding
 * masquerading
 * rate limiting

m| Command                                       | Explanation                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `man 5 firewalld.richlanguage`                | Displays the manual page for the rich language used to create complex firewall rules in firewalld.|
| `firewall-cmd --add-rich-rule='<rule>'`       | Adds a rich rule to the firewalld configuration.                                                  |
| `less /etc/protocols`                         | Displays the contents of the `/etc/protocols` file, which lists network protocols and their numbers. |

The purpose of NAT is to have a private network (eg: `10.0.0.0/24`) to protect it from outside and inside traffic. In NAT, network addresses are translated, and IP includes MAC addresses. When a package is sent, it provides a source and a destination IP, and the package is forwarded by the NAT by updating the source IP using the one in the NAT. The package includes back a MAC address, so when the response comeback, it is forwarded to the requestor MAC address. The routing table has the translation of MAC addreeses to IP addresses in the virtual network.

| Command                                                                                         | Explanation                                                                                       |
|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `firewall-cmd --zone=public --add-masquerade --permanent`                                       | Enables masquerading (NAT) in the `public` zone permanently.                                      |
| `firewall-cmd --zone=internal --change-interface=ens34`                                         | Changes the zone of the `ens34` interface to `internal`.                                          |
| `firewall-cmd --zone=internal --add-interface=ens33`                                            | Adds the `ens33` interface to the `internal` zone.                                                |
| `firewall-cmd --zone=public --add-forward-port=port=2022:proto=tcp:toport=22:toaddr=10.0.0.11`  | Forwards port 2022 (TCP) in the `public` zone to port 22 on the IP address `10.0.0.11`.           |

## UFW

| Command                                     | Explanation                                                                                       |
|---------------------------------------------|---------------------------------------------------------------------------------------------------|
| `ufw enable`                                | Enables the Uncomplicated Firewall (UFW).                                                         |
| `ufw status`                                | Displays the current status and rules of UFW.                                                     |
| `ufw allow ssh`                             | Allows SSH connections through the firewall.                                                      |
| `ufw reject out ssh`                        | Rejects outgoing SSH connections.                                                                 |
| `ufw delete reject out ssh`                 | Deletes the rule rejecting outgoing SSH connections.                                              |
| `ufw deny proto tcp from <ip> to any port <port>`| Denies TCP connections from the specified IP to any specified port.                               |
| `ufw reset`                                 | Resets UFW to its default state, removing all rules.                                              |
| `less /etc/services`                        | Displays the contents of the `/etc/services` file, which lists network services and their port numbers. |
| `ufw app list`                              | Lists all applications with profiles available for UFW.                                           |
| `ufw app info <app>`                        | Displays information about the specified application profile.                                     |
| `ufw logging on`                            | Enables logging for UFW.                                                                          |
| `man ufw`                                   | Displays the manual page for UFW, providing detailed information about the command and its options.|
