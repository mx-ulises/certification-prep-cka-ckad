## Running the ssh server

The Secure Shell (ssh) provides remote access to a linux terminal, and need to be installed and started. In some cases, you need to open ports in the firewall to allow traffic in on port 22.

| Command                                   | Explanation                                                                                       |
|-------------------------------------------|---------------------------------------------------------------------------------------------------|
| `dnf install openssh-server`              | Installs the OpenSSH server package using the `dnf` package manager.                              |
| `apt install openssh-server`              | Installs the OpenSSH server package using the `apt` package manager.                              |
| `systemctl status sshd`                   | Displays the current status and recent log entries of the SSH daemon.                             |
| `systemctl enable --now sshd`             | Enables the SSH daemon to start at boot and starts it immediately.                                |
| `systemctl start sshd`                    | Starts the SSH daemon immediately.                                                                |


It is commont to be a common target for brute force attacks. It can be configured to make it more secure by modifying `/etc/ssh/sshd_config`:
 * `Port`: defines the port on which SSH is listening
 * `PermitRootLogin`: helps to enable or disable root login.
 * `AllowedUsers`: Specifies a whitelist that are allowed to log in, by default it is everyone.
 * `PasswordAuthentication`: allows or disallows password-based logins.

On Red Hat family, SELinux will not allow SSH to run on a non-default port and this needs to be configured. To open the fireweall on Red Hat:

| Command                                                                    | Explanation                                                                                       |
|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `semanage port -a -t ssh_port_t -p tcp 2022`                               | Adds port 2022 as a TCP port type `ssh_port_t` for SELinux.                                       |
| `firewall-cmd --list-all`                  | Lists all the current firewall rules and settings.                                                |
| `firewall-cmd --add-service=ssh --permanent; firewall-cmd --reload`        | Adds the SSH service to the firewall permanently and reloads the firewall configuration.          |
| `firewall-cmd --add-port=2022/tcp --permanent; firewall-cmd --reload`      | Adds port 2022/tcp to the firewall permanently and reloads the firewall configuration.            |

You can also disable SELinux by making `SELINUX=disabled` in `/etc/sysconfig/selinux` and reboot.

To open the firewall on Ubuntu:

| Command                          | Explanation                                                                                       |
|----------------------------------|---------------------------------------------------------------------------------------------------|
| `ufw allow OpenSSH`              | Allows incoming connections for the OpenSSH service through the UFW firewall.                     |
| `ufw allow 2022/tcp`             | Allows incoming TCP connections on port 2022 through the UFW firewall.                            |

## Using the ssh client

When you first login into a ssh server, the server host key is cached in the `~/.ssh/known_hosts` files. While connecting to a host, the key is checked, and if it doesn't match, connection is refused.

| Command                                     | Explanation                                                                                       |
|---------------------------------------------|---------------------------------------------------------------------------------------------------|
| `ssh student@192.168.29.143`                | Initiates an SSH connection to the host at IP address 192.168.29.143 using the default SSH port (22) with the username `student`. |
| `ssh student@192.168.29.143`                | (Duplicate) Initiates an SSH connection to the host at IP address 192.168.29.143 using the default SSH port (22) with the username `student`. |
| `ssh -p 2022 student@192.168.29.143`        | Initiates an SSH connection to the host at IP address 192.168.29.143 using port 2022 with the username `student`. |

## Configure key-based ssh login

By default, ssh uses user and password pairs for providing access. For enhanced security, you can use key-based access. The user creates a key pair, and the public key is copied over to the ssh host where it is stored in the `~/.ssh/authorized_keys` file. For additional protection, the key may be protected with a passphrase, that would be asked every time session is started, ssh agent can cache passphrase for the session duration.

| Command                                    | Explanation                                                                                       |
|--------------------------------------------|---------------------------------------------------------------------------------------------------|
| `ssh-keygen`                               | Generates a new SSH key pair for secure authentication.                                           |
| `ssh-copy-id sshserver`                    | Copies the public SSH key to the specified SSH server for key-based authentication.               |
| `ssh-copy-id -p 2022 192.168.29.143`       | Copies the public SSH key to the server at IP address 192.168.29.143 on port 2022 for key-based authentication. |
| `ssh-agent`                                | Starts the SSH agent, which manages SSH keys and passwords.                                       |
| `ssh-agent /bin/bash`                      | Starts a new shell with the SSH agent environment.                                                |
| `ssh-add`                                  | Adds private SSH keys to the SSH agent for use in authentication.                                 |
