# Mysql_info 

Gather information about MySQL servers



## Synopsis

- Gathers information about MySQL servers.



## Requirements

The below requirements are needed on the host that executes this module.

- PyMySQL (Python 2.7 and Python 3.X), or
- MySQLdb (Python 2.x)



## Parameters

| Parameter                                                    | Choices/Defaults         | Comments                                                     |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| **ca_cert**                                                    path |                          | The path to a Certificate  Authority (CA) certificate. This option, if used, must specify the same  certificate as used by the server.                                                             aliases: ssl_ca |
| **client_cert**                                                    path |                          | The path to a client public key certificate.                                                             aliases: ssl_cert |
| **client_key**                                                    path |                          | The path to the client private key.                                                             aliases: ssl_key |
| **config_file**                                                    path | **Default:** "~/.my.cnf" | Specify a config file from which user and password are to be read. |
| **connect_timeout**                                                    integer | **Default:** 30          | The connection timeout when connecting to the MySQL server.  |
| **filter**                                                    list                     / elements=string |                          | Limit the collected information by comma separated string or YAML list.                                        Allowable values are `version`, `databases`, `settings`, `users`, `slave_status`, `slave_hosts`, `master_status`, `engines`.                                        By default, collects all subsets.                                        You can use '!' before value (for example, `!settings`) to exclude it from the information.                                        If you pass including and excluding values to the filter, for example, *filter=!settings,version*, the excluding values, `!settings` in this case, will be ignored. |
| **login_db**                                                    string |                          | Database name to connect to.                                        It makes sense if *login_user* is allowed to connect to a specific database only. |
| **login_host**                                                    string | **Default:** "localhost" | Host running the database.                                   |
| **login_password**                                                    string |                          | The password used to authenticate with.                      |
| **login_port**                                                    integer | **Default:** 3306        | Port of the MySQL server. Requires *login_host* be defined as other than localhost if login_port is used. |
| **login_unix_socket**                                                    string |                          | The path to a Unix domain socket for local connections.      |
| **login_user**                                                    string |                          | The username used to authenticate with.                      |



## Notes

- Requires the PyMySQL (Python 2.7 and Python 3.X) or MySQL-python  (Python 2.X) package on the remote host. The Python package may be  installed with apt-get install python-pymysql (Ubuntu; see [apt](https://docs.ansible.com/ansible/2.9/modules/apt_module.html#apt-module)) or yum install python2-PyMySQL (RHEL/CentOS/Fedora; see [yum](https://docs.ansible.com/ansible/2.9/modules/yum_module.html#yum-module)). You can also use dnf install python2-PyMySQL for newer versions of Fedora; see [dnf](https://docs.ansible.com/ansible/2.9/modules/dnf_module.html#dnf-module).
- Both `login_password` and `login_user` are required when you are passing credentials. If none are present, the module will attempt to read the credentials from `~/.my.cnf`, and finally fall back to using the MySQL default login of ‘root’ with no password.

## Examples

```ini
# Display info from mysql-hosts group (using creds from ~/.my.cnf to connect):
# ansible mysql-hosts -m mysql_info

# Display only databases and users info:
# ansible mysql-hosts -m mysql_info -a 'filter=databases,users'

# Display only slave status:
# ansible standby -m mysql_info -a 'filter=slave_status'

# Display all info from databases group except settings:
# ansible databases -m mysql_info -a 'filter=!settings'

- name: Collect all possible information using passwordless root access
  mysql_info:
    login_user: root

- name: Get MySQL version with non-default credentials
  mysql_info:
    login_user: mysuperuser
    login_password: mysuperpass
    filter: version

- name: Collect all info except settings and users by root
  mysql_info:
    login_user: root
    login_password: rootpass
    filter: "!settings,!users"

- name: Collect info about databases and version using ~/.my.cnf as a credential file
  become: yes
  mysql_info:
    filter:
    - databases
    - version

- name: Collect info about databases and version using ~alice/.my.cnf as a credential file
  become: yes
  mysql_info:
    config_file: /home/alice/.my.cnf
    filter:
    - databases
```