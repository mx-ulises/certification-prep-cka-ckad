## LDAP Configuration

Ligthweigth Directory Access Protocol (LDAP) is a generic solution to store important information centrally. LDAP is often used for authentication, but it can also store other information. LDAP server ports include 389 (insecure) and 363 (secured). In modern LDAP, Kerberos tickets are commonly used along with FreeIPA.

Identity, Policy and Authorization (IPA) is a commonly used LDAP server (FreeIPA usually). Can be installed on many distributions and used as a container. Administration happens trhought LDAP utilities such as `ipa` or web interface. It integrates with DNS.

| Command                                                                                               | Explanation                                                                                           |
|-------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| `hostnamectl set-hostname freeipa.example.local`                                                      | Sets the hostname to `freeipa.example.local`.                                                          |
| `vim /etc/hosts`                                                                                      | Edits the `/etc/hosts` file to add necessary host mappings.                                            |
| `dnf update`                                                                                          | Updates all installed packages to their latest versions.                                               |
| `dnf install -y ipa-server bind-dyndb-ldap ipa-server-dns`                                            | Installs the FreeIPA server, LDAP dynamic DNS, and DNS components.                                     |
| `ipa-server-install --setup-dns`                                                                      | Starts the FreeIPA server installation with DNS configuration.                                         |
| `firewall-cmd --permanent --add-service http`                                                         | Permanently allows HTTP service through the firewall.                                                  |
| `firewall-cmd --permanent --add-service https`                                                        | Permanently allows HTTPS service through the firewall.                                                 |
| `firewall-cmd --permanent --add-service ldap`                                                         | Permanently allows LDAP service through the firewall.                                                  |
| `firewall-cmd --permanent --add-service ldaps`                                                        | Permanently allows LDAPS (LDAP over SSL) service through the firewall.                                 |
| `firewall-cmd --permanent --add-service kerberos`                                                     | Permanently allows Kerberos service through the firewall.                                              |
| `firewall-cmd --permanent --add-service kpasswd`                                                      | Permanently allows Kerberos password service through the firewall.                                     |
| `firewall-cmd --permanent --add-service dns`                                                          | Permanently allows DNS service through the firewall.                                                   |
| `firewall-cmd --permanent --add-service ntp`                                                          | Permanently allows NTP (Network Time Protocol) service through the firewall.                           |
| `firewall-cmd --reload`                                                                               | Reloads the firewall to apply the new settings.                                                        |

After the IPA server is configured, users can be added and accessed. OpenLDAP comes with tools such as `ldapsearch` and `ldapadd` that allows direct communication with the LDAP server, and FreeIPA comes with the `ipa` tool, which provides an easy-to-use interface, as well as the client is installed already in the machine. LDAP clients allows to connect and fetch information from LDAP server.

| Command                                                                                                               | Explanation                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| `kinit admin`                                                                                                         | Initializes a Kerberos ticket for the `admin` user.                                                                        |
| `klist`                                                                                                               | Lists the current Kerberos tickets held by the user.                                                                       |
| `ipa user-add <user>`                                                                                                 | Adds a new user to the FreeIPA directory. Replace `<user>` with the desired username.                                      |
| `ipa passwd <user>`                                                                                                   | Sets or changes the password for the specified user in FreeIPA. Replace `<user>` with the username.                        |
| `ldapsearch -x -H ldap://<ip> -b "dc=example,dc=local" -s sub "objectclass=*"`                                         | Performs an anonymous LDAP search on the FreeIPA server at the specified IP. Replace `<ip>` with the server's IP address.  |
| `su - <user>`                                                                                                         | Switches to the specified user account. Replace `<user>` with the username.                                                |
| `cat /etc/sssd/sssd.conf`                                                                                             | Displays the contents of the `sssd.conf` file, which is used by the System Security Services Daemon (SSSD) for configuration. |
