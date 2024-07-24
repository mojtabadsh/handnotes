# Mysql_replication 

# Manage MySQL replication

## Synopsis

- Manages MySQL server replication, slave, master status get and change master host.

## Requirements

The below requirements are needed on the host that executes this module.

- PyMySQL (Python 2.7 and Python 3.X), or
- MySQLdb (Python 2.x)

## Parameters

| Parameter                                                    | Choices/Defaults                                             | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **ca_cert**                                                    path |                                                              | The path to a Certificate  Authority (CA) certificate. This option, if used, must specify the same  certificate as used by the server.                                                             aliases: ssl_ca |
| **client_cert**                                                    path |                                                              | The path to a client public key certificate.                                                             aliases: ssl_cert |
| **client_key**                                                    path |                                                              | The path to the client private key.                                                             aliases: ssl_key |
| **config_file**                                                    path | **Default:** "~/.my.cnf"                                     | Specify a config file from which user and password are to be read. |
| **connect_timeout**                                                    integer | **Default:** 30                                              | The connection timeout when connecting to the MySQL server.  |
| **login_host**                                                    string | **Default:** "localhost"                                     | Host running the database.                                   |
| **login_password**                                                    string |                                                              | The password used to authenticate with.                      |
| **login_port**                                                    integer | **Default:** 3306                                            | Port of the MySQL server. Requires *login_host* be defined as other than localhost if login_port is used. |
| **login_unix_socket**                                                    string |                                                              | The path to a Unix domain socket for local connections.      |
| **login_user**                                                    string |                                                              | The username used to authenticate with.                      |
| **master_auto_position**                                                    boolean | **Choices:**                                                                                                                                                                                            no                                                                                                                                                                                                                            yes | Whether the host uses GTID based replication or not.         |
| **master_connect_retry**                                                    integer |                                                              | Same as mysql variable.                                      |
| **master_host**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_log_file**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_log_pos**                                                    integer |                                                              | Same as mysql variable.                                      |
| **master_password**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_port**                                                    integer |                                                              | Same as mysql variable.                                      |
| **master_ssl**                                                    boolean | **Choices:**                                                                                                                                                                                            no                                                                                                                                                                                                                            yes | Same as mysql variable.                                      |
| **master_ssl_ca**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_ssl_capath**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_ssl_cert**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_ssl_cipher**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_ssl_key**                                                    string |                                                              | Same as mysql variable.                                      |
| **master_user**                                                    string |                                                              | Same as mysql variable.                                      |
| **mode**                                                    string | **Choices:**                                                                                                                                                                                            **getslave** ←                                                                                                                                                                                                                            getmaster                                                                                                                                                                                                                            changemaster                                                                                                                                                                                                                            stopslave                                                                                                                                                                                                                            startslave                                                                                                                                                                                                                            resetslave                                                                                                                                                                                                                            resetslaveall | module operating mode.  Could be getslave (SHOW SLAVE STATUS), getmaster (SHOW MASTER STATUS),  changemaster (CHANGE MASTER TO), startslave (START SLAVE), stopslave  (STOP SLAVE), resetslave (RESET SLAVE), resetslaveall (RESET SLAVE ALL) |
| **relay_log_file**                                                    string |                                                              | Same as mysql variable.                                      |
| **relay_log_pos**                                                    integer |                                                              | Same as mysql variable.                                      |



## Notes

- Requires the PyMySQL (Python 2.7 and Python 3.X) or MySQL-python  (Python 2.X) package on the remote host. The Python package may be  installed with apt-get install python-pymysql (Ubuntu; see [apt](https://docs.ansible.com/ansible/2.9/modules/apt_module.html#apt-module)) or yum install python2-PyMySQL (RHEL/CentOS/Fedora; see [yum](https://docs.ansible.com/ansible/2.9/modules/yum_module.html#yum-module)). You can also use dnf install python2-PyMySQL for newer versions of Fedora; see [dnf](https://docs.ansible.com/ansible/2.9/modules/dnf_module.html#dnf-module).
- Both `login_password` and `login_user` are required when you are passing credentials. If none are present, the module will attempt to read the credentials from `~/.my.cnf`, and finally fall back to using the MySQL default login of ‘root’ with no password.

## Examples

```ini
# Stop mysql slave thread
- mysql_replication:
    mode: stopslave

# Get master binlog file name and binlog position
- mysql_replication:
    mode: getmaster

# Change master to master server 192.0.2.1 and use binary log 'mysql-bin.000009' with position 4578
- mysql_replication:
    mode: changemaster
    master_host: 192.0.2.1
    master_log_file: mysql-bin.000009
    master_log_pos: 4578

# Check slave status using port 3308
- mysql_replication:
    mode: getslave
    login_host: ansible.example.com
    login_port: 3308
```