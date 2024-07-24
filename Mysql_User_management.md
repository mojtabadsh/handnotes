## Create a Database and User

In this section, we will create a database and user in MySQL.

First, connect to the MySQL shell using the following command:

```bash
$ mysql
```

Once you are connected, create a database named testdb and testdb1 using the following command:

```mysql
CREATE DATABASE testdb;
CREATE DATABASE testdb1;
```

Next, display all the databases using the following command:

```mysql
show databases;
```

Next, create a user named `testuser` for localhost and set a password using the following command:

```mysql
CREATE USER 'negar'@'localhost' IDENTIFIED BY 'password';
```

We have created a testuser for localhost, which means testuser will be able to connect to MySQL only from the localhost.

If you want to create a MySQL user and grant access from the remote machine with IP **192.168.10.100**, run the following command:

```mysql
CREATE USER 'testuser'@'192.168.10.100' IDENTIFIED BY 'password';
```

If you want to create a MySQL user and grant access from all remote hosts, run the following command:

```mysql
CREATE USER 'negar'@'%' IDENTIFIED BY 'BGT246tgb';
```

## Grant Privileges to a MySQL User Account

MySQL provides several types of user privileges that you can grant to a user. Some of them are listed below:

- **ALL PRIVILEGES:** Used to grant all privileges to the user account.
- **INSERT:** This allows the user to insert rows into a table.
- **SELECT:** Allows users to read a database.
- **UPDATE:** Allows users to update table rows.
- **CREATE:** Allows users to create a database and table.
- **DELETE:** This allows the user to delete rows from a table.
- **DROP:** Allows users to delete a database and table.

Let’s see some examples:

To grant all the privileges to **testuser** on **testdb** database, run the following command:

```mysql
GRANT ALL PRIVILEGES ON negar.* TO 'testuser'@'localhost';
```

To grant all the privileges to **testuser** on all **databases**, run the following command:

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'testuser'@'localhost';
```

To grant only SELECT, INSERT and DELETE privileges to **testuser** on **testdb1** database, run the following command:

```mysql
GRANT SELECT, INSERT, DELETE ON testdb1.* TO testuser@'localhost';
```

In the final

```mysql
flush privileges ;
```

Check users:

```mysql
show user from mysql.user
```

**Example**

```mysql
CREATE USER 'assetuser'@'%' IDENTIFIED BY 'DatxAsset%1';
GRANT SELECT ON ASSET_DB_PASARGAD_BACKUP.* TO 'assetuser'@'%';
flush privileges;

GRANT ALL PRIVILEGES ON Exchange_Hub2.* TO 'exhub2adapteruser'@'%';
ALTER USER 'exhub2adapteruser'@'%' IDENTIFIED BY 'exhub2adapterp@ss';

```



## Show Granted Privileges

You can use the SHOW GRANTS command to display the privileges that you have granted to MySQL users.

Run the following command to display all granted privileges to testuser:

```mysql
SHOW GRANTS FOR 'testuser'@'localhost';
```



## Revoke Privileges from a MySQL User Account

If you want to revoke or restore all privileges from a MySQL user over a database, use the revoke command as shown below:

```mysql
REVOKE ALL PRIVILEGES ON testdb.* FROM 'testuser'@'localhost';
```

## Delete MySQL Database and User Account

To delete a MySQL database, use the following command:

```mysql
DROP DATABSE testdb;
```

To delete a MySQL user account, use the following command:

```mysql
DROP USER 'testuser'@'localhost';
```

## change password

```mysql
ALTER USER 'apiuser'@'%' IDENTIFIED BY 'Apip@ss123';
flush privileges;
```

## Check Variable like validate_password_

```mysql
SHOW VARIABLES LIKE 'validate_password%';
show VARIABLES like "%password%"
```

## Change password policy to LOW or 0, 1 or Medium, 2 or strong

```mysql
set global validate_password_policy=0;
```

## Change password length

```mysql
set global validate_password_length=0;
```
