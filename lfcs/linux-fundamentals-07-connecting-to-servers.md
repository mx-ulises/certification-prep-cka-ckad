## Using root user priviledges

The `root` user exists in Linux kernel space and has no restrictions on what they can do. No need permisions to do whatever they need to use. This account is unlimited, and using this account should be avoided. Some distributions set no password on this account so it cannot be used, or make it optional.

An alternative is to use `sudo` which requires specific setup. You can use `su` to open a shell as a `root` user as an alternative, but only works if you have a password for `root`.

The command `su` is used to switch user, to open a shell as an specified user. The `root` user can change to any other user without prompting password. Regular users can also use `su` but they need to use the target user password.

| Command | Description                                                                                           |
|---------|-------------------------------------------------------------------------------------------------------|
| `su`    | Switch user to another user account. Requires the password of the target account.                      |
| `su -`  | Switch user to another user account and start a login shell. Requires the password of the target account. |
| `whoami`| Display the username of the current user.                                                              |

The command `sudo` is the preferred way to run administrative tasks. It is more sofisticated way to grant users proviledges to run specific tasks or all tasks with elevated permissions. The default admin user has `sudo` priviledges by default. To become a `sudo` user you need to be member of one of these groups:
 * On Red Hat, you need to be part of the group `wheel`
 * On Ubuntu, you need to be part of the group `sudo`

| Command     | Description                                                                                               |
|-------------|-----------------------------------------------------------------------------------------------------------|
| `sudo`      | Execute a command as another user, typically the superuser (root). Requires the user's password.          |
| `sudo -i`   | Start a new login shell as the superuser (root). Requires the user's password.                            |
| `sudo su -` | Switch to the superuser (root) account and start a login shell. Requires the user's password.             |
| `id`        | Display user identity information, including user ID (UID), group ID (GID), and group memberships.        |
| `exit`      | Exit the current shell or terminate the current user session.                                              |

To configure `sudo` access, use `visudo` to open the `/etc/sudoers` configuration file using the default configuration. You can add drop-in files to `/etc/sudoers.d`:

```
%wheel  ALL=(ALL)   ALL
%wheel  ALL=(ALL)   NOPASSWD: ALL
```

Add the following to cache valid sudo credentials for 4 hours:

```
Defaults timestamp_type=global,timestamp_timeout=240
```

You can add priviledges to use some commands to users with this type of configuration:

```
<user>  localhost=/usr/bin/passwd, /usr/sbin/useradd
```

By adding `!` as a preffix, this is going to prevent the user to run that command.

```
<user>  localhost=!/usr/bin/passwd
```

## Using ssh

The command `ssh` provide a secure shell to have remote access. It requires the server running `sshd` and the client using `ssh`. It replaces `telnet` which was used to establish remote unencrypted connections. The command `scp` helps to copy files from the local computer to a remote server (and viceversa).

| Command                                                                        | Description                                                                                                          |
|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| `sudo apt install openssh-server`                                              | Install the OpenSSH server package on a Debian-based system using `apt`.                                             |
| `sudo dnf install openssh-server; sudo systemctl enable --now sshd`            | Install the OpenSSH server package on a Red Hat-based system using `dnf`, and then enable and start the SSH service. |
| `scp /etc/hosts 192.168.29.11:/tmp/`                                           | Securely copy the `/etc/hosts` file to the `/tmp/` directory on a remote machine with IP address `192.168.29.11`.    |
| `scp /etc/hosts user@192.168.29.11:/tmp/`                                      | Securely copy the `/etc/hosts` file to the `/tmp/` directory on a remote machine with IP address `192.168.29.11` as a specific user. |
