# Mysql_db 

Add or remove MySQL databases from a remote host

## Synopsis

- Add or remove MySQL databases from a remote host.

## Requirements

The below requirements are needed on the host that executes this module.

- MySQLdb (Python 2.x)
- PyMySQL (Python 2.7 and Python 3.X), or
- mysql (command line binary)
- mysqldump (command line binary)



## Parameters

| Parameter                                                    | Choices/Defaults                                             | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | :----------------------------------------------------------- |
| **ca_cert**                                                    path |                                                              | The path to a Certificate  Authority (CA) certificate. This option, if used, must specify the same  certificate as used by the server.                                                             aliases: ssl_ca |
| **client_cert**                                                    path |                                                              | The path to a client public key certificate.                                                             aliases: ssl_cert |
| **client_key**                                                    path |                                                              | The path to the client private key.                                                             aliases: ssl_key |
| **collation**                                                    - |                                                              | Collation mode (sorting).  This only applies to new table/databases and does not update existing  ones, this is a limitation of MySQL. |
| **config_file**                                                    path | **Default:** "~/.my.cnf"                                     | Specify a config file from which user and password are to be read. |
| **connect_timeout**                                                    integer | **Default:** 30                                              | The connection timeout when connecting to the MySQL server.  |
| **encoding**                                                    - |                                                              | Encoding mode to use, examples include `utf8` or `latin1_swedish_ci` |
| **ignore_tables**                                                    -                                                                                added in 2.7 | **Default:** []                                              | A list of table names that will be ignored in the dump of the form database_name.table_name |
| **login_host**                                                    string | **Default:** "localhost"                                     | Host running the database.                                   |
| **login_password**                                                    string |                                                              | The password used to authenticate with.                      |
| **login_port**                                                    integer | **Default:** 3306                                            | Port of the MySQL server. Requires *login_host* be defined as other than localhost if login_port is used. |
| **login_unix_socket**                                                    string |                                                              | The path to a Unix domain socket for local connections.      |
| **login_user**                                                    string |                                                              | The username used to authenticate with.                      |
| **name**                                                    list                     / elements=string                         / required |                                                              | name of the database to add or remove.                                        *name=all* May only be provided if *state* is `dump` or `import`.                                        List of databases is provided with *state=dump*, *state=present* and *state=absent*.                                        if name=all Works like --all-databases option for mysqldump (Added in 2.0).                                                             aliases: db |
| **quick**                                                    boolean | **Choices:**                                                                                                                                                                                            no                                                                                                                                                                                                                            **yes** ← | Option used for dumping large tables                         |
| **single_transaction**                                                    boolean | **Choices:**                                                                                                                                                                                            **no** ←                                                                                                                                                                                                                            yes | Execute the dump in a single transaction                     |
| **state**                                                    - | **Choices:**                                                                                                                                                                                            **present** ←                                                                                                                                                                                                                            absent                                                                                                                                                                                                                            dump                                                                                                                                                                                                                            import | The database state                                           |
| **target**                                                    - |                                                              | Location, on the remote host, of the dump file to read from or write to. Uncompressed SQL files (`.sql`) as well as bzip2 (`.bz2`), gzip (`.gz`) and xz (Added in 2.0) compressed files are supported. |



## Notes

- Requires the mysql and mysqldump binaries on the remote host.
- This module is **not idempotent** when *state* is `import`, and will import the dump file each time if run more than once.
- Requires the PyMySQL (Python 2.7 and Python 3.X) or MySQL-python  (Python 2.X) package on the remote host. The Python package may be  installed with apt-get install python-pymysql (Ubuntu; see [apt](https://docs.ansible.com/ansible/2.9/modules/apt_module.html#apt-module)) or yum install python2-PyMySQL (RHEL/CentOS/Fedora; see [yum](https://docs.ansible.com/ansible/2.9/modules/yum_module.html#yum-module)). You can also use dnf install python2-PyMySQL for newer versions of Fedora; see [dnf](https://docs.ansible.com/ansible/2.9/modules/dnf_module.html#dnf-module).
- Both `login_password` and `login_user` are required when you are passing credentials. If none are present, the module will attempt to read the credentials from `~/.my.cnf`, and finally fall back to using the MySQL default login of ‘root’ with no password.



## Examples

```ini
- name: Create a new database with name 'bobdata'
  mysql_db:
    name: bobdata
    state: present

- name: Create new databases with names 'foo' and 'bar'
  mysql_db:
    name:
      - foo
      - bar
    state: present

# Copy database dump file to remote host and restore it to database 'my_db'
- name: Copy database dump file
  copy:
    src: dump.sql.bz2
    dest: /tmp

- name: Restore database
  mysql_db:
    name: my_db
    state: import
    target: /tmp/dump.sql.bz2

- name: Dump multiple databases
  mysql_db:
    state: dump
    name: db_1,db_2
    target: /tmp/dump.sql

- name: Dump multiple databases
  mysql_db:
    state: dump
    name:
      - db_1
      - db_2
    target: /tmp/dump.sql

- name: Dump all databases to hostname.sql
  mysql_db:
    state: dump
    name: all
    target: /tmp/dump.sql

- name: Import file.sql similar to mysql -u <username> -p <password> < hostname.sql
  mysql_db:
    state: import
    name: all
    target: /tmp/dump.sql

- name: Delete database with name 'bobdata'
  mysql_db:
    name: bobdata
    state: absent

- name: Make sure there is neither a database with name 'foo', nor one with name 'bar'
  mysql_db:
    name:
      - foo
      - bar
    state: absent
```