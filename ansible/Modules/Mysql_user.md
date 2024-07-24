# Mysql_user 

Adds or removes a user from a MySQL database

## Synopsis

- Adds or removes a user from a MySQL database.

## Requirements

The below requirements are needed on the host that executes this module.

- PyMySQL (Python 2.7 and Python 3.X), or
- MySQLdb (Python 2.x)

## Parameters

| Parameter                                                    | Choices/Defaults                                             | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **append_privs**                                                    boolean | **Choices:**                                                                                                                                                                                            **no** ←                                                                                                                                                                                                                            yes | Append the privileges defined by priv to the existing ones for this user instead of overwriting existing ones. |
| **ca_cert**                                                    path |                                                              | The path to a Certificate  Authority (CA) certificate. This option, if used, must specify the same  certificate as used by the server.                                                             aliases: ssl_ca |
| **check_implicit_admin**                                                    boolean | **Choices:**                                                                                                                                                                                            **no** ←                                                                                                                                                                                                                            yes | Check if mysql allows login as root/nopassword before trying supplied credentials. |
| **client_cert**                                                    path |                                                              | The path to a client public key certificate.                                                             aliases: ssl_cert |
| **client_key**                                                    path |                                                              | The path to the client private key.                                                             aliases: ssl_key |
| **config_file**                                                    path | **Default:** "~/.my.cnf"                                     | Specify a config file from which user and password are to be read. |
| **connect_timeout**                                                    integer | **Default:** 30                                              | The connection timeout when connecting to the MySQL server.  |
| **encrypted**                                                    boolean | **Choices:**                                                                                                                                                                                            **no** ←                                                                                                                                                                                                                            yes | Indicate that the 'password' field is a `mysql_native_password` hash. |
| **host**                                                    string | **Default:** "localhost"                                     | The 'host' part of the MySQL username.                       |
| **host_all**                                                    boolean | **Choices:**                                                                                                                                                                                            **no** ←                                                                                                                                                                                                                            yes | Override the host option, making ansible apply changes to all hostnames for a given user.                                        This option cannot be used when creating users. |
| **login_host**                                                    string | **Default:** "localhost"                                     | Host running the database.                                   |
| **login_password**                                                    string |                                                              | The password used to authenticate with.                      |
| **login_port**                                                    integer | **Default:** 3306                                            | Port of the MySQL server. Requires *login_host* be defined as other than localhost if login_port is used. |
| **login_unix_socket**                                                    string |                                                              | The path to a Unix domain socket for local connections.      |
| **login_user**                                                    string |                                                              | The username used to authenticate with.                      |
| **name**                                                    string                                             / required |                                                              | Name of the user (role) to add or remove.                    |
| **password**                                                    string |                                                              | Set the user's password..                                    |
| **priv**                                                    string |                                                              | MySQL privileges string in the format: `db.table:priv1,priv2`.                                        Multiple privileges can be specified by separating each one using a forward slash: `db.table:priv/db.table:priv`.                                        The format is based on MySQL `GRANT` statement.                                        Database and table names can be quoted, MySQL-style.                                        If column privileges are used, the `priv1,priv2` part must be exactly as returned by a `SHOW GRANT` statement. If not followed, the module will always report changes. It includes grouping columns by permission (`SELECT(col1,col2`) instead of `SELECT(col1`,SELECT(col2))). |
| **sql_log_bin**                                                    boolean | **Choices:**                                                                                                                                                                                            no                                                                                                                                                                                                                            **yes** ← | Whether binary logging should be enabled or disabled for the connection. |
| **state**                                                    string | **Choices:**                                                                                                                                                                                            absent                                                                                                                                                                                                                            **present** ← | Whether the user should exist.                                        When `absent`, removes the user. |
| **update_password**                                                    string | **Choices:**                                                                                                                                                                                            **always** ←                                                                                                                                                                                                                            on_create | `always` will update passwords if they differ.                                        `on_create` will only set the password for newly created users. |



## Notes

- MySQL server installs with default login_user of ‘root’ and no  password. To secure this user as part of an idempotent playbook, you  must create at least two tasks: the first must change the root user’s  password, without providing any login_user/login_password details. The  second must drop a ~/.my.cnf file containing the new root credentials.  Subsequent runs of the playbook will then succeed by reading the new  credentials from the file.
- Currently, there is only support for the mysql_native_password encrypted password hash module.
- Requires the PyMySQL (Python 2.7 and Python 3.X) or MySQL-python  (Python 2.X) package on the remote host. The Python package may be  installed with apt-get install python-pymysql (Ubuntu; see [apt](https://docs.ansible.com/ansible/2.9/modules/apt_module.html#apt-module)) or yum install python2-PyMySQL (RHEL/CentOS/Fedora; see [yum](https://docs.ansible.com/ansible/2.9/modules/yum_module.html#yum-module)). You can also use dnf install python2-PyMySQL for newer versions of Fedora; see [dnf](https://docs.ansible.com/ansible/2.9/modules/dnf_module.html#dnf-module).
- Both `login_password` and `login_user` are required when you are passing credentials. If none are present, the module will attempt to read the credentials from `~/.my.cnf`, and finally fall back to using the MySQL default login of ‘root’ with no password.

## Examples

```ini
- name: Removes anonymous user account for localhost
  mysql_user:
    name: ''
    host: localhost
    state: absent

- name: Removes all anonymous user accounts
  mysql_user:
    name: ''
    host_all: yes
    state: absent

- name: Create database user with name 'bob' and password '12345' with all database privileges
  mysql_user:
    name: bob
    password: 12345
    priv: '*.*:ALL'
    state: present

- name: Create database user using hashed password with all database privileges
  mysql_user:
    name: bob
    password: '*EE0D72C1085C46C5278932678FBE2C6A782821B4'
    encrypted: yes
    priv: '*.*:ALL'
    state: present

- name: Create database user with password and all database privileges and 'WITH GRANT OPTION'
  mysql_user:
    name: bob
    password: 12345
    priv: '*.*:ALL,GRANT'
    state: present

# Note that REQUIRESSL is a special privilege that should only apply to *.* by itself.
- name: Modify user to require SSL connections.
  mysql_user:
    name: bob
    append_privs: yes
    priv: '*.*:REQUIRESSL'
    state: present

- name: Ensure no user named 'sally'@'localhost' exists, also passing in the auth credentials.
  mysql_user:
    login_user: root
    login_password: 123456
    name: sally
    state: absent

- name: Ensure no user named 'sally' exists at all
  mysql_user:
    name: sally
    host_all: yes
    state: absent

- name: Specify grants composed of more than one word
  mysql_user:
    name: replication
    password: 12345
    priv: "*.*:REPLICATION CLIENT"
    state: present

- name: Revoke all privileges for user 'bob' and password '12345'
  mysql_user:
    name: bob
    password: 12345
    priv: "*.*:USAGE"
    state: present

# Example privileges string format
# mydb.*:INSERT,UPDATE/anotherdb.*:SELECT/yetanotherdb.*:ALL

- name: Example using login_unix_socket to connect to server
  mysql_user:
    name: root
    password: abc123
    login_unix_socket: /var/run/mysqld/mysqld.sock

- name: Example of skipping binary logging while adding user 'bob'
  mysql_user:
    name: bob
    password: 12345
    priv: "*.*:USAGE"
    state: present
    sql_log_bin: no

# Example .my.cnf file for setting the root password
# [client]
# user=root
# password=n<_665{vS43y
```

## Status

- This module is not guaranteed to have a backwards compatible interface. *[preview]*
- This module is [maintained by the Ansible Community](https://docs.ansible.com/ansible/2.9/user_guide/modules_support.html#modules-support). *[community]*